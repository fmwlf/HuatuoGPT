# HuatuoGPT (华佗GPT), Towards Taming Language Models To Be a Doctor.

## ✨ Latest News
- [05/25/2023]: Release the [tech report](https://arxiv.org/pdf/2305.15075.pdf) and the HuatuoGPT [demo](https://www.huatuogpt.cn/).

## ⚡ Introduction
Welcome to the repository of HuatuoGPT, a large language model (LLM) trained on a vast Chinese medical corpus. Our objective with HuatuoGPT is to construct a more professional ‘ChatGPT’ for medical consultation scenarios. 
欢迎来到华托GPT的知识库，这是一个在庞大的中国医学语料库上训练的大型语言模型。我们与华托GPT的目标是为医疗咨询场景构建一个更专业的“ChatGPT”。

Here is a list of what has been released:
以下是已发布的内容列表：

1. HuatuoGPT-SFT-data: A hybrid SFT data capitalizing on both strengths to endow the model with Doctor-like and Patient-friendly characteristics.
2. HuatuoGPT model: HuatuoGPT model weights and the online demo.
3. Medical evaluation benchmark: an evaluation method used to evaluate LLMs in medical scenarios.
1.华托GPT SFT数据：一个混合的SFT数据，利用这两种优势，赋予该模型类似医生和患者友好的特征。
2.华托GPT模型：华托GPt模型权重及在线演示。
3.医疗评估基准：用于评估医疗场景中LLM的评估方法。

<div align=center>
<img src="assets/huatuo.png" width = "640" alt="HuatuoGPT" align=center/>
</div>


## 💭 Motivation
- To address the growing demand for quick medical consultations both online and in hospitals that do not necessarily require deep medical knowledge. We believe that LLMs like HuatuoGPT can be effectively utilized to meet these demands, freeing up physicians’ time and energy for more complex cases.
- To provide open data for training medical LLMs. Building high-quality instruction training data for LLMs is essential, but it can be also challenging. We have constructed medical instruction data using various methods and made it publicly available. This dataset can be combined with other datasets to train one's own medical 'ChatGPT'.
- To emphasize the importance of carefully evaluating the ability of medical LLMs before using them to offer medical assistance to patients. We recognize the potential benefits of LLMs in the medical field, but also acknowledge the need for thorough evaluation and testing to ensure patient safety and accurate diagnoses.
-为了满足日益增长的在线和医院快速医疗咨询需求，这些需求不一定需要深入的医学知识。我们相信，像华托GPT这样的LLM可以有效地满足这些需求，为更复杂的病例腾出医生的时间和精力。
-为医学LLM培训提供开放数据。为LLM构建高质量的教学培训数据至关重要，但也可能具有挑战性。我们使用各种方法构建了医学指导数据，并将其公开。该数据集可以与其他数据集相结合，以训练自己的医学“ChatGPT”。
-强调在使用医疗LLM为患者提供医疗援助之前，仔细评估其能力的重要性。我们认识到LLM在医疗领域的潜在好处，但也认识到需要进行彻底的评估和测试，以确保患者的安全和准确的诊断。

## 📚 Data

### Overview

To leverage the best of both distilled data (from ChatGPT) and real-world data (from Doctors), we firstly fine-tune  `HuatuoGPT`  using four types of data:

| Type                          | Scale | Descriptions                                                 | Examples                                                     |
| ------------------------------------ | ----- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Distilled Instructions from ChatGPT  | 61,400   | Medical instructions generated with self-instruct. Seed instructions are manually written by us. | 💬：现在该做什么准备手术前的身体检查？<br>👨‍⚕️：在准备手术前的身体检查中，需要进行以下几个步骤：<br>&emsp;&emsp;1. 了解病人的病史：了解病人的病史，包括既往病史、手术史、药物过敏史、家族病史等，以便评估手术风险和制定手术方案。<br>&emsp;&emsp;2. 进行体格检查：进行全面的体格检查，包括测量身高、体重、血压、心率、呼吸… |
| Real-world Instructions from Doctors | 69,768  | Medical single-round medical consultants in real-world scenarios, in which the doctor's response is more specific and can address the user's medical concerns. | 💬：我有糖尿病血压160-90.该吃什么降压药？空腹血糖8左右。餐后11左右。我有糖尿病血压160-90.该吃什么降压药？<br>👨‍⚕️：阅读你的提问描述，综合分析症状，体征，糖尿病，血压160-90.血压略高，空腹血糖8左右。餐后11左右。血糖略高。口服依那普利，二甲双胍片治疗，新近研究，二甲双胍片，可以抑制食欲，促进肠道蠕动，预防癌症，预防心脑血管病，延长寿命作用。 |
| Distilled Conversations from ChatGPT     | 68,888   | Medical multi-turn conversations are created using two ChatGPTs: one acting as the patient and the other as the doctor. The conversations are based on real-world patient cases and their corresponding treatment. | 💬：User: 我想问一下大夫，我发烧三天后，第四天全身起了疹子，是怎么回事呢？<br>👨‍⚕️：~您好！感谢您的提问。您的症状可能与幼儿急疹相关。幼儿急疹是一种常见的儿童病，病因为人类疱疹病毒6型或7型感染引起。它的症状包括：<br> 1. 发热 n2. 全身起红色疹子 <br> 3. 口腔内及咽喉部有小水泡<br> 4. 病程一般为3-5天左右...<br>💬：谢谢大夫！除了幼儿急疹还有其他可能吗？我之前有感冒，这和疹子有关系吗？<br>👨‍⚕️：除了幼儿急疹，病毒性感冒和其他病毒感染也有可能引起疹子，但这种情况相对较少见。在您的情况下，感冒可能是一种诱发因素，但主要还是由病毒感染引起的疹子... |
| Real-world Conversations with Doctors      | 25,986   | Medical multi-round conversations in real-world scenarios. Conversations include multiple rounds of questioning by doctors and continuous questioning by patients. | 💬：额头下巴下长满痘痘。就是不知道.<br>👨‍⚕️：你好，有什么困扰吗？我可以为你提供帮助。请问您平时的饮食习惯、睡眠和护肤习惯怎样？化妆品和防晒霜的使用频率如何？还有洁面的频次是怎样的呢？<br>💬：平时我基本上只是用水乳霜，然后洗面奶只用一种。日常防晒只有在夏天时会使用，我一般用悦诗风吟的洗面奶以及AHC的护肤产品，这些都比较温和。<br>👨‍⚕️：谢谢您的描述。首先，建议您停用化妆品和普通护肤品，使用特定的药妆品牌，如雅漾和理肤泉的洁面泡沫或洁面乳，并避免使用热水或肥皂清洗面部。建议您外用阿达帕林凝胶涂于面部痤疮，一般夜间使用，涂抹十五分钟之后外用医学护肤品比如雅漾、薇资、理肤泉清爽型的舒缓面霜，或者是维生素e乳膏… |

### Download
- [HuatuoGPT-sft-data-v1](https://huggingface.co/datasets/FreedomIntelligence/HuatuoGPT-sft-data-v1): The data used in the Supervised Fine-Tuning (SFT) stage of HuatuoGPT.

  

## 👨‍⚕️ Model

### Model Access

- [HuatuoGPT-v1](https://huggingface.co/FreedomIntelligence/HuatuoGPT-7b-v1) (Currently unavailable, coming soon.)

### Deploy

Firstly, you should install all required packages
```bash
pip install -r requirements.txt
```

Please make sure you have download our model weights and run
```bash
python -m huatuo_cli_demo_stream.py --model-name $model_dir
```



## 🚀 Demo

Try our model in [https://www.huatuogpt.cn/](https://www.huatuogpt.cn/). Note that it is still in progressing.

<!-- ![demo_1](assets/demo_1.png) -->
<!-- ![demo_2](assets/demo_2.png) -->



## 🧐 Evaluations

### Automatic Evaluation Using GPT-4

<div align=center>
<img src="assets/eval1.png"  alt="eval1" align=center/>
</div>

### Expert  Evaluation

* Single-turn question

|  | Win | Loss |
| --------------------------- | :--: | :--: |
| **HuatuoGPT** vs. DoctorGLM         |  98%  |  2%  |
| **HuatuoGPT** vs. ChatGPT       |  **52%**  |  48%  |

* Multi-turn diagnosis

|  | Win | Loss |
| --------------------------- | :--: | :--: |
| **HuatuoGPT** vs. DoctorGLM         |  86%  |  14%  |
| **HuatuoGPT** vs. ChatGPT       |  **58%**  |  42%  |



## 🤖 Limitations

Our goal with HuatuoGPT is to address the need for quick medical consultations, rather than replace doctors or provide full medical support to patients. However, our model does have several limitations that must be taken into consideration:

- Misunderstandings: As with all language models, there is a risk of misunderstandings or misinterpretations, especially when dealing with medical jargon or complex conditions. In this scenario, our models may give wrong answers.
- Hallucinations: Large language models can sometimes generate responses that do not make sense or are completely unrelated to the given input. These "hallucinations" can be especially problematic when users are not familiar with the concepts being discussed, as they may not be able to easily recognize the errors in the model's output. These "hallucinations" can be a challenge to detect and avoid.
- Bias: LLMs are trained on large datasets, which can inadvertently introduce bias into the model's responses. Additionally, care should be taken to ensure that the model is not used to perpetuate biases in medical treatment.

## Acknowledgement

We are aware that our works are inspired by the following works, including but not limited to

- Bloom: https://huggingface.co/bigscience/bloom
- Self-instruct: https://github.com/yizhongw/self-instruct

Without these, nothing could happen in this repository.

## Citation
```angular2
@article{huatuogpt-2023,
  title={HuatuoGPT, Towards Taming Language Models To Be a Doctor},
  author={Hongbo Zhang and Junying Chen and Feng Jiang and Fei Yu and Zhihong Chen and Jianquan Li and Guiming Chen and Xiangbo Wu and Zhiyi Zhang and Qingying Xiao and Xiang Wan and Benyou Wang and Haizhou Li},
  journal={arXiv preprint arXiv:2305.15075},
  year={2023}
}
```

We are from the School of Data Science, the Chinese University of Hong Kong, Shenzhen (CUHKSZ) and the Shenzhen Rsearch
Institute of Big Data (SRIBD).

## Star History

<a href="https://star-history.com/#FreedomIntelligence/HuatuoGPT&Date">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=FreedomIntelligence/HuatuoGPT&type=Date&theme=dark" />
    <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=FreedomIntelligence/HuatuoGPT&type=Date" />
    <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=FreedomIntelligence/HuatuoGPT&type=Date" />
  </picture>
</a>
