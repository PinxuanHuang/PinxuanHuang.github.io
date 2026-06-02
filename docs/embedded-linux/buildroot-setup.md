---
sidebar_position: 1
title: Buildroot 環境建置與自訂分割區
sidebar_label: Buildroot 基礎
---

# Buildroot 環境建置與自訂分割區紀錄

在開發自訂的內嵌式 Linux 韌體時，常需要調整分割區以優化儲存空間。以下紀錄在 Ubuntu x86 宿主機上的設定重點。

## 1. 基礎環境依賴安裝

在編譯前，確保系統已安裝必要套件：

```bash
sudo apt-get install build-essential libncurses5-dev rsync unzip bc
```
