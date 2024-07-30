# Clone and Package Hugging Face Model GitHub Action / 克隆和打包Hugging Face模型的GitHub Action 

This GitHub Action clones a specified git repository, packages it into a tar file, and uploads the tar file as an artifact for download. 
此GitHub Action会克隆指定的git仓库，将其打包成tar文件，并将生成的tar文件作为可供下载的压缩包上传。

## Usage / 使用方法

To use this action, you need to fork this repository 
要使用此action，你需要fork这个存储库
or
或者
add a workflow file to your repository. The workflow file should be located at `.github/workflows/clone_and_package.yml`.  
在你的存储库中添加一个工作流文件，该工作流应位于`.github/workflows`

### workflow

```yaml 
name: Clone and Package Model

on:
  workflow_dispatch:
    inputs:
      git_url:
        description: 'Git URL to clone'
        required: true
        default: 'https://huggingface.co/immich-app/XLM-Roberta-Large-Vit-B-16Plus'

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repository
      uses: actions/checkout@v2

    - name: Install git-lfs
      run: |
        sudo apt-get install git-lfs
        git lfs install

    - name: Clone Hugging Face model
      run: |
        git clone ${{ github.event.inputs.git_url }} model
        tar -czvf model.tar.gz model

    - name: Upload tar file as artifact
      uses: actions/upload-artifact@v2
      with:
        name: model-tar
        path: model.tar.gz
​⬤
