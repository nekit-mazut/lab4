# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #4 - "Перцептрон" выполнил:
- Дюпин Никита Александрович
- РИ-220936

Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | # | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
В проекте Unity реализовать перцептрон.

## Задание 1
### В проекте Unity реализовать перцептрон, который умеет производить вычисления: OR, AND, NAND, XOR, дать комментарии о корректности работы.
Ход работы:  
 * был создан пустой 3D проект Unity.  
 * был скачан скрипт ```Perceptron.cs``` и добавлен пустому объекту perceptron.  
   Содержание скрипта:
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class TrainingSet
{
	public double[] input;
	public double output;
}

public class Perceptron : MonoBehaviour {

	public TrainingSet[] ts;
	double[] weights = {0,0};
	double bias = 0;
	double totalError = 0;

	double DotProductBias(double[] v1, double[] v2) 
	{
		if (v1 == null || v2 == null)
			return -1;
	 
		if (v1.Length != v2.Length)
			return -1;
	 
		double d = 0;
		for (int x = 0; x < v1.Length; x++)
		{
			d += v1[x] * v2[x];
		}

		d += bias;
	 
		return d;
	}

	double CalcOutput(int i)
	{
		double dp = DotProductBias(weights,ts[i].input);
		if(dp > 0) return(1);
		return (0);
	}

	void InitialiseWeights()
	{
		for(int i = 0; i < weights.Length; i++)
		{
			weights[i] = Random.Range(-1.0f,1.0f);
		}
		bias = Random.Range(-1.0f,1.0f);
	}

	void UpdateWeights(int j)
	{
		double error = ts[j].output - CalcOutput(j);
		totalError += Mathf.Abs((float)error);
		for(int i = 0; i < weights.Length; i++)
		{			
			weights[i] = weights[i] + error*ts[j].input[i]; 
		}
		bias += error;
	}

	double CalcOutput(double i1, double i2)
	{
		double[] inp = new double[] {i1, i2};
		double dp = DotProductBias(weights,inp);
		if(dp > 0) return(1);
		return (0);
	}

	void Train(int epochs)
	{
		InitialiseWeights();
		
		for(int e = 0; e < epochs; e++)
		{
			totalError = 0;
			for(int t = 0; t < ts.Length; t++)
			{
				UpdateWeights(t);
				Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
			}
			Debug.Log("TOTAL ERROR: " + totalError);
		}
	}

	void Start () {
		Train(8);
		Debug.Log("Test 0 0: " + CalcOutput(0,0));
		Debug.Log("Test 0 1: " + CalcOutput(0,1));
		Debug.Log("Test 1 0: " + CalcOutput(1,0));
		Debug.Log("Test 1 1: " + CalcOutput(1,1));		
	}
	
	void Update () {
		
	}
}
   ```  
 * далее "перцептрон" был обучен производить вычисление операции OR, для этого была заполнена таблица истинности соответствующяя OR (рис. 1)
   -![image](https://github.com/nekit-mazut/lab4/assets/145917921/2fd3ded2-15b8-4d3d-8d46-94f2989601b6)***рис. 1***
   - за 5 эпох обучения, перцептрон научился безошибочно производить операцию OR(рис. 2)
   - ![image](https://github.com/nekit-mazut/lab4/assets/145917921/1a7f3e46-1a4a-4b25-834a-d230d81f1a26)***рис. 2***  
 *далее "перцептрон" был обучен производить вычисление операции AND, для этого была заполнена таблица истинности AND (рис. 3)
   - ![image](https://github.com/nekit-mazut/lab4/assets/145917921/66c0e9f4-0af1-41d4-8408-cb220bc170bd)***рис. 3*** 
   - за 7 эпох обучения, перцептрон научился безошибочно производить операцию AND(рис. 4)
   - ![image](https://github.com/nekit-mazut/lab4/assets/145917921/998cf069-1f34-45f9-ac1d-d60351080537) ***рис. 4***   
 * далее "перцептрон" был обучен производить вычисление операции NAND, для этого была заполнена таблица истинности NAND (рис. 5)  
   - ![image](https://github.com/nekit-mazut/lab4/assets/145917921/69942b48-1f30-4a14-92fe-d31bac6e1082)***рис. 5***  
   - за 6 эпох обучения, перцептрон научился безошибочно производить операцию NAND(рис. 6)  
   - ![image](https://github.com/nekit-mazut/lab4/assets/145917921/220368cd-9ccd-4505-9005-f104d87e35bb)***рис. 6*** 
 * - далее "перцептрон" был обучен производить вычисление операции XOR, для этого была заполнена таблица истинности XOR(рис. 7)
   - ![image](https://github.com/nekit-mazut/lab4/assets/145917921/2b456bcc-a492-46de-9258-e013a9c67578)***рис. 7***
   - за 4 эпохи обучения, перцептрон научился безошибочно производить операцию XOR(рис. 8)
   -![image](https://github.com/nekit-mazut/lab4/assets/145917921/dc02649f-29e8-4b3f-bb2a-b15be502ea9d)***рис. 8***


## Задание 2  
### Построить графики зависимости количества эпох от ошибки обучения. Указать от чего зависит необходимое количество эпох обучения.  
![image](https://github.com/nekit-mazut/lab4/assets/145917921/d37ca576-94d8-4033-9777-4cb491d18cce)
-Оптимальное количество эпох — это область между состояниями недообучения и переобучения модели. Определяется это количество эмпирически в контексте конкретной задачи. А определить эту область можно с помощью усредненного графика минимизации функции потерь по результатам нескольких итераций обучения.

## Выводы
По итогу работы, я научился работать с перцептроном и анализировать данные на основе его работы. 

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
