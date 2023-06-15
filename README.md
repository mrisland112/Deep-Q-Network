# Deep-Q-Network
- 此做為 Q-learning 的進階應用，將深度神經網路導入結合 Q-learning 演算法，訓練約 2.5 hr

## 使用技術
- PyTorch 深度學習框架
- 構建卷積神經網路 CNN
- OpenAI Gym 建立遊戲環境
- 建立 Q-learning 演算法


## 目的
- 利用 Reinforcement learning 的方式，去訓練模型得到最大化獎勵，讓模型自行去學習球軌跡，回擊對手的乒乓球攻擊，最後先得到 21 分取得比賽勝利
- 每局的最大 total rewards 為 12

## 設立環境
1. 移除 setuptools 並重新安裝
```
# 因為遊戲 ROM 使用的 OpenAI Gym 版本較低
# 需要先對環境 setuptools 進行移除，選擇較舊的版本，再進行下一階段的套件安裝
pip uninstall setuptools
pip install setuptools==50.3.1
```
2. 安裝 PyTorch (使用最新版 PyTorch 2.0.1) 並取用 cuda 環境
- 其他版本可以到 https://pytorch.org/get-started/locally/ 進行查詢
```
# 安裝 PyTorch、CUDA
conda install pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia
```

```
# 確認 GPU 裝置啟用 
import torch
torch.cuda.is_available()
```

```
# 確認 GPU 裝置啟用 
import torch
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
print(device)
```
<img width="777" alt="torch" src="https://github.com/mrisland112/Deep-Q-Network/assets/28065019/c99826be-90bf-4501-af7f-6f4a43ae0adf">


3. ROM data
```
# 下載 ROM data 並解壓縮 (Linux, colab)
! wget http://www.atarimania.com/roms/Roms.rar
! mkdir /content/ROM/
! unrar e /content/Roms.rar /content/ROM/
! python -m atari_py.import_roms /content/ROM/

# 下載 ROM data 並解壓縮 (Windows)
! wget http://www.atarimania.com/roms/Roms.rar -OutFile ./Roms.rar
! mkdir ./ROM/
使用解壓縮軟體將 rar file 解壓縮到 ./ROM/
! python -m atari_py.import_roms ./ROM/
```
4. 安裝 OpenAI Gym
```
pip install gym==0.19.0                    #注意：gym 0.19.0版本最高只支援到python 3.9.x的環境，如果要使用則需要確定python環境符合需求
pip install gym[atari]                     #注意：使用此指令有時候會套件安裝不完全，會缺失一個檔案
conda install -c conda-forge atari_py      #可以改利用此指令可以解決套件安裝不完整問題(建議使用)
pip install numpy
pip install matplotlib
```

## 訓練過程
- 在初始訓練時，平均 total reward 偏低，代表模型處在一個不穩定狀態，模型(右方)很容易接不到對手(左方)回擊過來的乒乓球

![train_010000](https://github.com/mrisland112/Deep-Q-Network/assets/28065019/3a966191-a9ce-4c2d-8946-9cfa22b81464)

## 訓練結果
- 當訓練 episode 達到 170 次時，平均 total reward 達到收斂，模型(右方)可以去接下各種軌跡的球

![train_400000](https://github.com/mrisland112/Deep-Q-Network/assets/28065019/0578e8df-d5a0-47ef-b9ee-2382b9668498)


## 儲存訓練結果
- DQN.py 執行完會儲存 qnet.pt，在後續測試我們會使用到這個檔案

## 測試
- 可以發現訓練好的模型可以應付各種情況，達成接下各種軌跡的球的任務

![eval](https://github.com/Nashexplorer/Deep-Q-Network/assets/132718430/84bc14b7-f231-4b33-b750-eb48295d1b20)

