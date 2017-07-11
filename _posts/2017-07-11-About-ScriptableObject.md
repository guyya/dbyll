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
