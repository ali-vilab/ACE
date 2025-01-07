<p align="center">

  <h2 align="center"><img src="assets/figures/icon.png" height=16> : All-round Creator and Editor Following <br> Instructions via Diffusion Transformer</h2>

  <p align="center">
    <a href="https://arxiv.org/abs/2410.00086"><img src='https://img.shields.io/badge/arXiv-ACE-red' alt='Paper PDF'></a>
    <a href='https://ali-vilab.github.io/ace-page'><img src='https://img.shields.io/badge/Project_Page-ACE-blue' alt='Project Page'></a>
    <a href='https://github.com/modelscope/scepter'><img src='https://img.shields.io/badge/Scepter-ACE-green'></a>
    <a href='https://huggingface.co/spaces/scepter-studio/ACE-Chat'><img src='https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Space-orange'></a>
    <a href='https://huggingface.co/scepter-studio/ACE-0.6B-512px'><img src='https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Model-orange'></a>
    <a href='https://www.modelscope.cn/models/iic/ACE-0.6B-512px'><img src='https://img.shields.io/badge/ModelScope-Model-purple'></a>
    <br>
    <strong>Zhen Han*</strong>
    ·
    <strong>Zeyinzi Jiang*</strong>
    ·
    <strong>Yulin Pan*</strong>
    ·
    <strong>Jingfeng Zhang*</strong>
    ·
    <strong>Chaojie Mao*</strong>
    <br>
    <strong>Chenwei Xie</strong>
    ·
    <strong>Yu Liu</strong>
    ·
    <strong>Jingren Zhou</strong>
    <br>
    Tongyi Lab, Alibaba Group
  </p>
  <table align="center">
    <tr>
    <td>
      <img src="assets/figures/teaser.png">
    </td>
    </tr>
  </table>

## 📢 News
* **[2024.9.30]** Release the paper of ACE on arxiv.
* **[2024.10.31]** Release the ACE checkpoint on [ModelScope](https://www.modelscope.cn/models/iic/ACE-0.6B-512px) and [HuggingFace](https://huggingface.co/scepter-studio/ACE-0.6B-512px).
* **[2024.11.1]** Support online demo on [HuggingFace](https://huggingface.co/spaces/scepter-studio/ACE-Chat).
* **[2024.11.20]** Release the [ACE-0.6b-1024px](https://huggingface.co/scepter-studio/ACE-0.6B-1024px) model, 
which significantly enhances image generation quality compared with [ACE-0.6b-512px](https://huggingface.co/scepter-studio/ACE-0.6B-512px).
* **[2025.01.06]** Release the [ACE++](https://ali-vilab.github.io/ACE_plus_page/).


## 🚀 Installation
Install the necessary packages with `pip`: 
```bash
pip install -r requirements.txt
```

##  🔥 ACE Models
|    **Model**     |                                                                                                                                                                                                            **Status**                                                                                                                                                                                                             | 
|:----------------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
|  ACE-0.6B-512px  |          [![Demo link](https://img.shields.io/badge/Demo-ACE_Chat-purple)](https://huggingface.co/spaces/scepter-studio/ACE-Chat)<br>[![ModelScope link](https://img.shields.io/badge/ModelScope-Model-blue)](https://www.modelscope.cn/models/iic/ACE-0.6B-512px)  [![HuggingFace link](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Model-yellow)](https://huggingface.co/scepter-studio/ACE-0.6B-512px)          |
| ACE-0.6B-1024px  | [![ModelScope link](https://img.shields.io/badge/ModelScope-Model-blue)](https://www.modelscope.cn/models/iic/ACE-0.6B-1024px)  [![HuggingFace link](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Model-yellow)](https://huggingface.co/scepter-studio/ACE-0.6B-1024px) |             |
| ACE-12B-FLUX-dev |                                                                                                                                        The ACE model based on the FLUX.1-dev base model has adopted a new adaptation method. We have organized a new project called [ACE++](https://ali-vilab.github.io/ACE_plus_page/). The relevant models have been open-sourced. Please visit to learn more.                                                                                                                                               |             |

## 🖼 Model Performance Visualization

The current model's parameters scale of ACE is 0.6B, which imposes certain limitations on the quality of image generation. [FLUX.1-Dev](https://huggingface.co/black-forest-labs/FLUX.1-dev), on the other hand, 
has a significant advantage in text-to-image generation quality. By using SDEdit, we can effectively leverage the generative capabilities of FLUX to further enhance the image results generated by ACE. Based on the above considerations, we have designed the ACE-Refiner pipeline, as shown in the diagram below.

![ACE_REFINER](assets/ace_method/ace_refiner_process.webp)

As shown in the figure below, when the strength 
σ of the generated image is high, the generated image will suffer from fidelity loss compared to the original image. Conversely, lower 
σ does not significantly improve the image quality. Therefore, users can make a trade-off between fidelity to the generated result and the image quality based on their own needs. 
Users can set the value of "REFINER_SCALE" in the configuration file `config/inference_config/models/ace_0.6b_1024_refiner.yaml`. 
We recommend that users use the advance options in the [webui-demo](#-chat-bot-) for effect verification.

![ACE_REFINER_EXAMPLE](assets/ace_method/ace_refiner.webp)


We compared the generation and editing performance of different models on several tasks, as shown as following. 
![Samples](assets/ace_method/samples_compare.webp)


## 🔥 Training

We offer a demonstration training YAML that enables the end-to-end training of ACE using a toy dataset. For a comprehensive overview of the hyperparameter configurations, please consult `config/ace_0.6b_512_train.yaml`.

### Prepare datasets

Please find the dataset class located in `modules/data/dataset/dataset.py`, 
designed to facilitate end-to-end training using an open-source toy dataset. 
Download a dataset zip file from [modelscope](https://www.modelscope.cn/models/iic/scepter/resolve/master/datasets/hed_pair.zip), and then extract its contents into the `cache/datasets/` directory.

Should you wish to prepare your own datasets, we recommend consulting `modules/data/dataset/dataset.py` for detailed guidance on the required data format.

### Prepare initial weight
The ACE checkpoint has been uploaded to both ModelScope and HuggingFace platforms:
* [ModelScope](https://www.modelscope.cn/models/iic/ACE-0.6B-512px)
* [HuggingFace](https://huggingface.co/scepter-studio/ACE-0.6B-512px)

In the provided training YAML configuration, we have designated the Modelscope URL as the default checkpoint URL. Should you wish to transition to Hugging Face, you can effortlessly achieve this by modifying the PRETRAINED_MODEL value within the YAML file (replace the prefix "ms://iic" to "hf://scepter-studio").


### Start training

You can easily start training procedure by executing the following command:
```bash
# ACE-0.6B-512px
PYTHONPATH=. python tools/run_train.py --cfg config/ace_0.6b_512_train.yaml
# ACE-0.6B-1024px
PYTHONPATH=. python tools/run_train.py --cfg config/ace_0.6b_1024_train.yaml
```

## 🚀 Inference

We provide a simple inference demo that allows users to generate images from text descriptions.
```bash
 PYTHONPATH=. python tools/run_inference.py --cfg config/inference_config/models/ace_0.6b_512.yaml --instruction "make the boy cry, his eyes filled with tears" --seed 199999 --input_image examples/input_images/example0.webp
```
We recommend running the examples for quick testing. Running the following command will run the example inference and the results will be saved in `examples/output_images/`.
```bash
PYTHONPATH=. python tools/run_inference.py --cfg config/inference_config/models/ace_0.6b_512.yaml
```

## 💬 Chat Bot 
We have developed an chatbot UI utilizing Gradio, designed to transform user input in natural language into visually stunning images that align semantically with the provided instructions. Users can effortlessly initiate the chatbot app by executing the following command:
```bash
python chatbot/run_gradio.py --cfg chatbot/config/chatbot_ui.yaml --server_port 2024
```

<table align="center">
  <tr>
  <td>
    <img src="assets/videos/demo_chat.gif">
  </td>
  </tr>
</table>

## ⚙️️ ComfyUI Workflow

![Workflow](assets/comfyui/ace_example.jpg)

We support the use of ACE in the ComfyUI Workflow through the following methods:

1) Automatic installation directly via the ComfyUI Manager by searching for the **ComfyUI-Scepter** node.
2) Manually install by moving custom_nodes from Scepter to ComfyUI.
```shell
git clone https://github.com/modelscope/scepter.git
cd path/to/scepter
pip install -e .
cp -r path/to/scepter/workflow/ path/to/ComfyUI/custom_nodes/ComfyUI-Scepter
cd path/to/ComfyUI
python main.py
```

**Note**: You can use the nodes by dragging the sample images below into ComfyUI. Additionally, our nodes can automatically pull models from ModelScope or HuggingFace by selecting the *model_source* field, or you can place the already downloaded models in a local path.

<table><tbody>
  <tr>
    <th align="center" colspan="4">ACE Workflow Examples</th>
  </tr>
  <tr>
    <th align="center" colspan="1">Control</th>
    <th align="center" colspan="1">Semantic</th>
    <th align="center" colspan="1">Element</th>
  </tr>
  <tr>
    <td>
      <a href="assets/comfyui/ace_control.png" target="_blank">
        <img src="assets/comfyui/ace_control.png" width="200">
      </a>
    </td>
    <td>
      <a href="assets/comfyui/ace_semantic.png" target="_blank">
        <img src="assets/comfyui/ace_semantic.png" width="200">
      </a>
    </td>
    <td>
      <a href="assets/comfyui/ace_element.png" target="_blank">
        <img src="assets/comfyui/ace_element.png" width="200">
      </a>
    </td>
  </tr>
</tbody>
</table>


## 📝 Citation

```bibtex
@article{han2024ace,
  title={ACE: All-round Creator and Editor Following Instructions via Diffusion Transformer},
  author={Han, Zhen and Jiang, Zeyinzi and Pan, Yulin and Zhang, Jingfeng and Mao, Chaojie and Xie, Chenwei and Liu, Yu and Zhou, Jingren},
  journal={arXiv preprint arXiv:2410.00086},
  year={2024}
}
```
