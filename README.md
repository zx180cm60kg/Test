using UnityEngine;
using System.Collections;

public class yzx_controller : MonoBehaviour
{
    SteamVR_TrackedObject Hand;
    SteamVR_Controller.Device device;

    bool IsShock = false;  //布尔型变量IsShock

    // Use this for initialization
    void Start()
    {
        Hand = GetComponent();  //获得SteamVR_ TrackedObject组件  
    }

    // Update is called once per frame
    void Update()
    {

        //防止Start函数没加载成功，保证SteamVR_ TrackedObject组件获取成功！
        if (Hand.isValid)
        {
            Hand = GetComponent();
        }
        device = SteamVR_Controller.Input((int)Hand.index);    //根据index，获得手柄 
                                                               //如果手柄的Trigger键被按下了
        if (device.GetPressDown(SteamVR_Controller.ButtonMask.Trigger))
        {

            IsShock = false;  //每次按下，IsShock为false,才能保证手柄震动
            StartCoroutine("Shock", 0.5f); //开启协程Shock(),第二个参数0.5f 即为协程Shock()的形参
        }

    }

    //定义了一个协程

    IEnumerator Shock(float durationTime)

    {

        //Invoke函数，表示durationTime秒后，执行StopShock函数；
        Invoke("StopShock", durationTime);

        //协程一直使得手柄产生震动，直到布尔型变量IsShock为false;
        while (!IsShock)
        {
            device.TriggerHapticPulse(500);

            yield return new WaitForEndOfFrame();

        }


    }

    void StopShock()
    {
        IsShock = true; //关闭手柄的震动
    }


}
