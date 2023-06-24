{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "accelerator": "GPU",
    "colab": {
      "provenance": [],
      "toc_visible": true,
      "machine_shape": "hm",
      "gpuType": "A100",
      "include_colab_link": true
    },
    "kernelspec": {
      "display_name": "Python 3",
      "name": "python3"
    },
    "gpuClass": "standard"
  },
  "cells": [
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "view-in-github",
        "colab_type": "text"
      },
      "source": [
        "<a href=\"https://colab.research.google.com/github/dream80/DeepFaceLab_Colab/blob/master/DeepFaceLab_Colab_V5.ipynb\" target=\"_parent\"><img src=\"https://colab.research.google.com/assets/colab-badge.svg\" alt=\"Open In Colab\"/></a>"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "KWQMLYlFrTXz"
      },
      "source": [
        "# 简介\n",
        "\n",
        "\n",
        "这是一个在线AI换脸的脚本，只需要用浏览器打开，点击相应的步骤即可完成视频换脸。具体操作参考视频教程。\n",
        "\n",
        "此脚本由**[托尼不是塔克](https://www.tonyisstark.com)**创建，仅用于学习，请勿滥用。如有疑问，拉到最后！\n",
        "\n",
        "视频教程：https://www.youtube.com/watch?v=MgcnGnfVDcc&list=PL4wPj6o65_XHcqBZ7iMVEaKrPi5-LrYNZ"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "AmGMbWZef1XI",
        "cellView": "form"
      },
      "source": [
        "#@title 查看分配到的设备\n",
        "# 查看设备，是K80还是T4,如果是K80...！\n",
        "! /opt/bin/nvidia-smi"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "ldpC3PjRrYYh"
      },
      "source": [
        "# 第一步 准备工作\n",
        "\n",
        "\n",
        "\n",
        "\n",
        " "
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "注意：  \n",
        "初始化项目root指定你自己路径，一般  \n",
        "云端硬盘路径为：/content/drive/MyDrive/  \n",
        "共享云端路径为：/content/drive/Shareddrives/DriverName  \n",
        "DriverName：共享盘名称，每个人不一样，根据自己情况来改。\n",
        "\n",
        "*准备素材不要乱点，在已经有素材和模型的情况下！！！"
      ],
      "metadata": {
        "id": "nc6Ycf1ymBsU"
      }
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "KtPP3K9inlLM",
        "cellView": "form"
      },
      "source": [
        "#@title 挂载云盘\n",
        "from google.colab import drive\n",
        "drive.mount('/content/drive')"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "ze6K9tYTkqK_",
        "cellView": "form",
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "a348859c-5bad-40c4-b88e-8a313d2ef9cf"
      },
      "source": [
        "#@title 初始化项目\n",
        "\n",
        "import os \n",
        "root = \"/content/drive/MyDrive/\" #@param {type:\"string\"}\n",
        "%cd $root\n",
        "!mkdir DeepFaceLab\n",
        "deepfacelab=os.path.join(root,\"DeepFaceLab\")\n",
        "workspace=os.path.join(deepfacelab,\"workspace\")\n",
        "deepfacelab_cloab=os.path.join(deepfacelab,\"DeepFaceLab_Colab\")\n",
        "\n"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "/content\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "cellView": "form",
        "id": "fptlIHykh3HU"
      },
      "source": [
        "#@title 准备素材（点击后会清空workspace）\n",
        "\n",
        "online = True #@param {type:\"boolean\"}\n",
        "upload = \"data_src/aligned\" #@param [\"data_src/aligned\",\"data_dst/aligned\",\"model\",\"data_src\",\"data_dst\"]\n",
        "\n",
        "if online :\n",
        "  %cd $deepfacelab\n",
        "  !rm -rf workspace\n",
        "  !git clone https://github.com/dream80/DFLWorkspace.git workspace\n",
        "else:\n",
        "  from google.colab import files\n",
        "  import shutil\n",
        "  upload_path = os.path.join(workspace,upload)\n",
        "  %cd $upload_path\n",
        "  #if os.path.isdir(upload_path):\n",
        "  #shutil.rmtree(upload_path)\n",
        "  uploaded = files.upload()\n",
        "  '''for filename in uploaded.keys():\n",
        "    print(basepath+\"/\"+filename)\n",
        "    print(upload_path+\"/\"+filename)\n",
        "    shutil.move(os.path.join(basepath, filename), os.path.join(upload_path, filename))\n",
        "  '''\n"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "9eiY_suIhUD-"
      },
      "source": [
        "# 第二部 安装软件"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "4CbWbLzHzqTQ",
        "cellView": "form"
      },
      "source": [
        "#@title 开始安装\n",
        "\n",
        "Version = \"v3.07.20.1\" #@param [\"v2.02.03\", \"v2.02.23b\", \"v2.02.28\",\"v2.03.07\",\"v2.03.15\",\"v2.06.01\",\"v2.06.19\",\"v2.08.02\",\"v3.01.04\",\"v3.07.20\",\"v3.07.20.1\",\"last\"]\n",
        "%cd $deepfacelab\n",
        "!rm -fr DeepFaceLab_Colab\n",
        "\n",
        "if Version==\"620\":\n",
        "  print(\"620版加载中....\")\n",
        "  # 获取DFL源代码v1.6.1稳定版\n",
        "  !git clone -b v1.6.1 https://github.com/dream80/DeepFaceLab_Colab.git\n",
        "  %cd $deepfacelab_cloab\n",
        "  !pip install -r requirements-colab.txt  \n",
        "elif Version==\"last\":\n",
        "  print(\"最新版加载中....\")\n",
        "  !git clone https://github.com/iperov/DeepFaceLab.git DeepFaceLab_Colab\n",
        "  %cd $deepfacelab_cloab\n",
        "  !pip install h5py\n",
        "  !pip install opencv-python\n",
        "  !pip install ffmpeg-python==0.1.17\n",
        "  !pip install colorama\n",
        "  !pip install tf2onnx==1.9.3\n",
        "else:\n",
        "  print(Version+\" 加载中....\")\n",
        "  cmd=\"clone -b \"+Version+\" https://github.com/dream80/DeepFaceLab_Colab.git\"  \n",
        "  !git $cmd\n",
        "  %cd $deepfacelab_cloab\n",
        "  !pip install h5py\n",
        "  !pip install opencv-python\n",
        "  !pip install ffmpeg-python==0.1.17\n",
        "  !pip install colorama\n",
        "  !pip install tf2onnx==1.9.3\n"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "9G9s5gJrty-x"
      },
      "source": [
        "# 第三步. 提取脸部"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "cellView": "form",
        "id": "YnhZyEERAW_D"
      },
      "source": [
        "#@title 开始提取SRC\n",
        "\n",
        "%cd $deepfacelab_cloab\n",
        "print(\"\\n===分解视频==================================\\n\")\n",
        "!python main.py videoed extract-video --input-file ../workspace/data_src.mp4 --output-dir ../workspace/data_src/\n",
        "print(\"\\n===提取人脸==================================\\n\")\n",
        "!python main.py extract --input-dir ../workspace/data_src --output-dir ../workspace/data_src/aligned --detector s3fd\n",
        "print(\"\\n===打包人脸==================================\\n\")\n",
        "!python main.py util --input-dir ../workspace/data_src/aligned  --pack-faceset\n"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "cellView": "form",
        "id": "VvdQB1XhuO8o"
      },
      "source": [
        "#@title 开始提取DST\n",
        "%cd $deepfacelab_cloab\n",
        "print(\"\\n===分解视频==================================\\n\")\n",
        "!python main.py videoed extract-video --input-file ../workspace/data_dst.mp4 --output-dir ../workspace/data_dst/\n",
        "print(\"\\n===提取人脸==================================\\n\")\n",
        "!python main.py extract --input-dir ../workspace/data_dst --output-dir ../workspace/data_dst/aligned --detector s3fd --output-debug\n",
        "print(\"\\n===打包人脸==================================\\n\")\n",
        "!python main.py util --input-dir ../workspace/data_dst/aligned  --pack-faceset\n"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "# 第四步. 训练模型"
      ],
      "metadata": {
        "id": "S_0L_wq6wrHN"
      }
    },
    {
      "cell_type": "code",
      "metadata": {
        "cellView": "form",
        "id": "MKQbOrIiyFJL"
      },
      "source": [
        "#@title 训练模型\n",
        "Model = \"SAEHD\" #@param [\"SAEHD\",\"primary\",\"AMP\",\"Quick96\",\"XSeg\"]\n",
        "%cd $deepfacelab_cloab\n",
        "cmd = \"main.py train --training-data-src-dir ../workspace/data_src/aligned --training-data-dst-dir ../workspace/data_dst/aligned --model-dir ../workspace/model --model \"+Model+\" --no-preview\"\n",
        "!python $cmd"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "cellView": "form",
        "colab": {
          "background_save": true
        },
        "id": "kEYnBHhJBZVs"
      },
      "source": [
        "#@title SRC-SRC\n",
        "Model = \"AMP\" #@param [\"SAEHD\",\"AMP\",\"Quick96\",\"XSeg\"]\n",
        "%cd $deepfacelab_cloab\n",
        "cmd = \"main.py train --training-data-src-dir ../workspace/data_src/aligned --training-data-dst-dir ../workspace/data_src/aligned --model-dir ../workspace/model --model \"+Model+\" --no-preview\"\n",
        "!python $cmd"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "m2Ba96cVuS0K"
      },
      "source": [
        "# 第五步. 转换输出\n",
        "\n"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "cellView": "form",
        "id": "G_oEgeU7I9jt"
      },
      "source": [
        "#@title 应用模型\n",
        "\n",
        "Model = \"SAEHD\" #@param [\"SAEHD\",\"primary\", \"Quick96\"]\n",
        "%cd $deepfacelab_cloab\n",
        "cmd = \" main.py merge --output-mask-dir ../workspace/data_dst/merged_mask --input-dir ../workspace/data_dst --output-dir ../workspace/data_dst/merged --aligned-dir ../workspace/data_dst/aligned --model-dir ../workspace/model --model \" + Model\n",
        "!python $cmd"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "cellView": "form",
        "id": "fbxfQ8UbJrqk"
      },
      "source": [
        "#@title 合成视频\n",
        "%cd $deepfacelab_cloab\n",
        "\n",
        "format = \"mp4\" #@param [\"mp4\",\"avi\",\"mov\"]\n",
        "lossless = False #@param {type:\"boolean\"}\n",
        "\n",
        "merged = \"main.py videoed video-from-sequence --input-dir ../workspace/data_dst/merged --output-file ../workspace/result.mp4 --reference-file ../workspace/data_dst.mp4 --include-audio\"\n",
        "merged_mask = \"main.py videoed video-from-sequence --input-dir ../workspace/data_dst/merged_mask --output-file ../workspace/result_mask.mp4 --reference-file ../workspace/data_dst.mp4 --include-audio --lossless\"\n",
        "\n",
        "\n",
        "if lossless :\n",
        "  merged=merged+\" --include-audio --lossless\"\n",
        "\n",
        "!python $merged\n",
        "!python $merged_mask\n",
        "\n"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "cVfp8xbY5ykL"
      },
      "source": [
        "#第六步. 继续训练\n",
        "\n"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "veL_8afb6WcP",
        "cellView": "form"
      },
      "source": [
        "#@title 一键运行\n",
        "#挂载谷歌云盘\n",
        "#点击链接授权，复制授权码，填入框框，然后回车。\n",
        "\n",
        "Model = \"SAEHD\" #@param [\"SAEHD\",\"AMP\",\"Quick96\",\"XSeg\"]\n",
        "\n",
        "#挂载\n",
        "from google.colab import drive\n",
        "drive.mount('/content/drive')\n",
        "\n",
        "import os \n",
        "root = \"/content/drive/MyDrive/\" #@param {type:\"string\"}\n",
        "\n",
        "%cd $root\n",
        "!mkdir DeepFaceLab\n",
        "deepfacelab=os.path.join(root,\"DeepFaceLab\")\n",
        "workspace=os.path.join(deepfacelab,\"workspace\")\n",
        "deepfacelab_cloab=os.path.join(deepfacelab,\"DeepFaceLab_Colab\")\n",
        "\n",
        "%cd $deepfacelab_cloab\n",
        "# 安装Python依赖\n",
        "!pip install h5py\n",
        "!pip install opencv-python\n",
        "!pip install ffmpeg-python==0.1.17\n",
        "!pip install colorama\n",
        "!pip install tf2onnx==1.9.3\n",
        "# 开始训练SAE ，如果是其他模型，修改后面的参数即可，比如H128。\n",
        "cmd = \"main.py train --training-data-src-dir ../workspace/data_src/aligned --training-data-dst-dir ../workspace/data_dst/aligned --model-dir ../workspace/model --model \"+Model+\" --no-preview\"\n",
        "!python $cmd "
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "siaPGJ1QVacj"
      },
      "source": [
        "# 工具\n"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "WGLTllAHYAPJ"
      },
      "source": [
        "\n",
        "打包素材。可以加快上传，下载，加载的速度。"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "cellView": "form",
        "id": "5MrM8JfIFiy7"
      },
      "source": [
        "#@title 开始排序\n",
        "target = \"src\" #@param [\"src\",\"dst\"]\n",
        "\n",
        "if target==\"src\":\n",
        "  #Src排序，可以通过谷歌云盘查看结果，删除不良图片\n",
        "  cmd = \"main.py sort --input-dir ../workspace/data_src/aligned\"\n",
        "  \n",
        "else:\n",
        "  cmd = \"main.py sort --input-dir ../workspace/data_dst/aligned \"\n",
        "\n",
        "!python $cmd"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "cellView": "form",
        "id": "fphcCxuh9Qfw"
      },
      "source": [
        "#@title 素材打包\n",
        "\n",
        "target = \"dst\" #@param [\"src\",\"dst\"]\n",
        "%cd $deepfacelab_cloab\n",
        "if target==\"src\":\n",
        "  !python main.py util --input-dir ../workspace/data_src/aligned  --pack-faceset\n",
        "else:\n",
        "  !python main.py util --input-dir ../workspace/data_dst/aligned  --pack-faceset\n",
        " \n",
        "  "
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "cellView": "form",
        "id": "bC2CyDDx6siX"
      },
      "source": [
        "#@title 素材解包\n",
        "\n",
        "target = \"src\" #@param [\"src\",\"dst\"]\n",
        "%cd $deepfacelab_cloab\n",
        "if target==\"src\":\n",
        "  !python main.py util --input-dir ../workspace/data_src/aligned --unpack-faceset\n",
        "else:\n",
        "  !python main.py util --input-dir ../workspace/data_dst/aligned --unpack-faceset\n",
        " \n",
        "  "
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "cellView": "form",
        "id": "NYDWkORS6xum"
      },
      "source": [
        "#@title 素材增强\n",
        "\n",
        "target = \"src\" #@param [\"src\"]\n",
        "%cd $deepfacelab_cloab\n",
        "if target==\"src\":\n",
        "  !python main.py facesettool enhance --input-dir ../workspace/data_src/aligned\n",
        "else:\n",
        "  !python main.py facesettool enhance --input-dir ../workspace/data_dst/aligned\n",
        " \n",
        "  "
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "5H85SsIW1PI3"
      },
      "source": [
        "# 其他\n",
        "\n",
        "\n",
        " 谷歌云地址：https://drive.google.com/drive/my-drive\n",
        "\n",
        " 作者邮箱 ：wpgdream@gmail.com\n",
        " \n",
        " 知识星球：https://t.zsxq.com/vFuzRNr\n",
        " \n",
        " Github ：https://github.com/dream80/DeepFaceLab_Colab  \n",
        " 有用就点个star 谢谢!!!  \n",
        "  \n"
      ]
    }
  ]
}
