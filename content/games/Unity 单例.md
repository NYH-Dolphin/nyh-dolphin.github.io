---
title: "Unity 单例"
date: 2022-04-06
draft: false
type: "post"
tags: ["Unity"]
---

## 1. Manager

很多全局控制的 GameManager 可以采取如下的单例模式以控制全局唯一

```c#
public class GameManager : MonoBehaviour
    {
        private static GameManager instance;

        public static GameManager Instance
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
}
```

- 即，创建一个 GameObject 上面挂在指定的 Manager 的脚本代码，然后使用 DontDestoryOnLoad 控制其始终在 Scene 上
