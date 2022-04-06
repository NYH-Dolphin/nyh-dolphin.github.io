---
title: "Unity 对象池"
date: 2022-04-06
draft: false
type: "post"
tags: ["Unity"]
---

## 1. ILSimplePool

```c#
using System.Collections.Generic;

namespace Util
{
    /// <summary>
    /// 简易池
    /// </summary>
    public class ILSimplePool<T>
    {
        protected List<T> listActive = new List<T>(); // 活跃的对象池
        protected List<T> listUnactive = new List<T>(); // 不活跃的对象池

        public List<T> ActiveInfos => listActive;
        public List<T> UnactiveInfos => listUnactive;

        public int ActiveCount => listActive.Count;
        public int UnactiveCount => listUnactive.Count;
        public T this[int nIndex] => listActive[nIndex];


        /// <summary>
        /// 从活跃池子内拿出一个对象
        /// </summary>
        /// <returns></returns>
        public T Pop()
        {
            T pRes = default(T);
            if (listUnactive.Count > 0)
            {
                pRes = listUnactive[0];
                listUnactive.RemoveAt(0);
                AddActiveObj(pRes);
            }

            return pRes;
        }

        /// <summary>
        /// 将对象添加到活跃池中
        /// </summary>
        /// <param name="pTarget"></param>
        public void AddActiveObj(T pTarget)
        {
            listActive.Add(pTarget);
        }

        /// <summary>
        /// 将活跃池中的指定对象放回不活跃池中
        /// </summary>
        /// <param name="pTarget"></param>
        public void RecycleObj(T pTarget)
        {
            bool isRemove = listActive.Remove(pTarget);
            if (isRemove)
            {
                listUnactive.Add(pTarget);
            }
        }

        /// <summary>
        /// 刷新整个池子
        /// </summary>
        public void Clear()
        {
            listActive.Clear();
            listUnactive.Clear();
        }
    }
}
```

存放指定某个对象（包含多个副本）的池子

- `listActive`：当前正在活跃的对象
- `listUnactive`：当前不在活跃的对象

## 2. Prefab Manager

```c#
public class PrefabManager : MonoBehaviour
    {
        private static PrefabManager instance;

        public static PrefabManager Instance
        {
            get
            {
                if (instance == null)
                {
                    GameObject obj = new GameObject();
                    obj.name = "PrefabManager";
                    DontDestroyOnLoad(obj);
                    instance = obj.AddComponent<PrefabManager>();
                }

                return instance;
            }
        }


        // 根据prefab地址字符串存放的object池
        private Dictionary<string, ILSimplePool<GameObject>> objPool =
            new Dictionary<string, ILSimplePool<GameObject>>();


        // 获得对象
        public GameObject GetObj(string objStr)
        {
            if (!objPool.ContainsKey(objStr))
            {
                objPool.Add(objStr, new ILSimplePool<GameObject>());
            }

            GameObject obj = objPool[objStr].Pop();
            if (obj == null)
            {
                obj = Instantiate((GameObject) Resources.Load(objStr));
                objPool[objStr].AddActiveObj(obj);
            }
            else
            {
                obj.SetActive(true);
            }
            return obj;
        }


        // 获得对象 - 重构版本 从对象池子获得object的同时为其设置位置和方向
        public GameObject GetObj(string objStr, Vector3 position, Vector3 eulerAngles)
        {
            if (!objPool.ContainsKey(objStr))
            {
                objPool.Add(objStr, new ILSimplePool<GameObject>());
            }

            GameObject obj = objPool[objStr].Pop();
            if (obj == null)
            {
                GameObject objI = (GameObject) Resources.Load(objStr);
                objI.transform.position = position;
                objI.transform.eulerAngles = eulerAngles;
                obj = Instantiate(objI);
                objPool[objStr].AddActiveObj(obj);
            }
            else
            {
                obj.transform.position = position;
                obj.transform.eulerAngles = eulerAngles;
                obj.SetActive(true);
            }
            return obj;
        }


        // 回收不用的对象
        public void ReturnObj(string objStr, GameObject obj)
        {
            if (string.IsNullOrEmpty(objStr))
            {
                return;
            }

            if (!objPool.ContainsKey(objStr))
            {
                objPool.Add(objStr, new ILSimplePool<GameObject>());
            }

            obj.SetActive(false);
            objPool[objStr].RecycleObj(obj);
        }
    }
```

- 当需要获取 object 的时候，以 object 的地址字符串传入即可获取
- 当需要回收 object 的时候，以 object 的地址字符串及 object 自己传入即可回收

