---
layout: post
title: About ScriptableObject (PartyFD)
categories: project
tags: [game, unity3d]
description: Data storage & ScriptableObject in Unity
---

유저가 Game을 진행하다보면 진행상황과 성장된 스탯등을 기록할 필요가 있다.
다시 게임을 시작했을 떄 리셋되지 않아야 할 데이타이다.

이러한 데이타는 모바일 디바이스의 어딘가에 파일 형태로 보관되거나 서버의 디비에 기록되어
보존해야 한다.

그러한 목적으로 사용할 수 있는 데이타를 unity에서는 아래와 같이 클래스로 정의하고 파일로
저장할 수 있다.

{% highlight c# %}
using System;

[System.Serialazable]
public class PlayerData
{
  public string name;
  public int level;
}

public class Data
{
    public static void Save<T>(T obj, string filename)
    {
        BinaryFormatter bf = new BinaryFormatter();
        //Application.persistentDataPath is a string, so if you wanted you can put that into debug.log if you want to know where save games are located
        FileStream file = File.Create(Application.persistentDataPath + filename); //you can call it anything you want
        bf.Serialize(file, obj);
        file.Close();
    }

    public static T Load<T>(string filename) where T : new()
    {
        string path = Application.persistentDataPath + filename;
        if (File.Exists(path))
        {
            BinaryFormatter bf = new BinaryFormatter();
            FileStream file = File.Open(path, FileMode.Open);

            T result = (T)bf.Deserialize(file);
            file.Close();

            return result;
        }

        //return default(T);
        return new T();
    }
}
{% endhighlight %}

위 코드에서 PlayerData 를 저장/로딩하려면 아래 처럼 하면 된다.

{% highlight c# %}
PlayerData data;

Data.Save<PlayerData>(data, "playerdata.gd");
data = Data.Load<PlayerData>("playerdata.gd");
{% endhighlight %}

??

그렇다면 ScriptableObject는 어떻게 쓰는 것일까? 처음에 UnityEditor상에서 ScriptableObject
를 상속받은 클래스 역시 마치 파일에 저장/로딩되는 것처럼 에디터상에서 수정된 값이 유지되고,
심지어 게임 플레이 도중의 값 역시 유지된다. (실제로는 그렇지 않다)

그래서 위의 PlayerData와 마치 동일하고 ScriptableObject로 게임데이타를 저장할 수 있지 않나
착각했었다. (적어도 필자는 그랬다. 마이 헷갈려;;)

하지만 결론은 다음과 같다.
* ScriptableObject는 에디터 상에서만 값이 유지된다.
* ScriptableObject는 게임 플레이 도중의 값을 저장하는 용도롤 쓸 수 없다.
* ScriptableObject는 개발자(기획자)가 기입하는 고정된 게임 데이타 이다.  

정확한 정의는 아니지만 "게임데이타"와 "플레이데이타"는 다르다.

* 게임데이타 : 게임을 위해서 개발자가 작성한 고정된 데이타
* 플레이데이타: 유저가 게임플레이로 만들어가는 변동적인 데이타

예를 들어 스킬을 예시로 할 수 있겠다.
