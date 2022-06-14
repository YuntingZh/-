# CutOff shader

**create -> shader graph -> URP ->lit shader graph （INSTEAD OF** shader ->PBR shader; The Lit graph is the same thing as the PBR graph**）**

if AlphaClip > Alpha, not showing

![](<.gitbook/assets/image (5).png>)

Smoothstep: create a edge gradient

![check both sides](<.gitbook/assets/image (2).png>)

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[RequireComponent(typeof(Renderer))]
public class CutOffEffect : MonoBehaviour
{
    [SerializeField] private float Cutoff = 1.0f;
    //range[-0.7, 0.8] 
    public Material material;
    private float speed = 0.5f;
    private float yAngle = 1;
    private void Awake()
    {
        material = GetComponent<Renderer>().material;
        this.transform.position = new Vector3(-1.8f, 1.6f, 0f);

    }

    private void Update()
    {
        var time = Time.time * Mathf.PI * speed;
        float var = transform.position.z;
        var += Mathf.Sin(time) * (Cutoff / 2.0f);
        SetVar(var);
        Debug.Log(var);
        this.transform.Rotate(0, yAngle, 0, Space.Self);

    }
    
    private void SetVar(float var)
    {
        material.SetFloat("_CutOff", var);

    }
}

```





