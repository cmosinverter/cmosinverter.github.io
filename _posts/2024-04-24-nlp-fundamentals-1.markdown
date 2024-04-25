---
layout:     post
title:      "NLP基礎學習 - Text Preprocessing"
subtitle:   "將文本轉換為標記的過程"
date:       2024-04-25
author:     "PCLiu"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
tags:
  - NLP
---


> 在自然語言處理中，Tokenization是將文本轉換為標記的過程。標記是文本中的最小單位，可以是單詞、字符、子詞等。Tokenization是自然語言處理的第一步，它將文本轉換為機器可讀的形式，為後續的處理提供了基礎。在本文中，我們將探討Tokenization的基本概念，常見的Tokenization方法，以及如何在Python中實現Tokenization。

## Tokenization

Tokenization 的形式有非常多種，目前大致可以分成三大類

### Character Tokenization

也就是直接把文本用字符作為最小單位進行分割，不考慮詞彙的意義。  
在Python裡面我們只要使用`list()`函數就可以將一個字符串轉換為字符列表。

```python
string = 'Tokenization is a crucial step in text processing where text is split into smaller units, such as words, phrases, or symbols.'
list(string)
```
可以得到輸出：
```
['T', 'o', 'k', 'e', 'n', 'i', 'z', 'a', 't', 'i', 'o', 'n', ' ', 'i', 's', ' ', 'a', ' ', 'c', 'r', 'u', 'c', 'i', 'a', 'l', ' ', 's', 't', 'e', 'p', ' ', 'i', 'n', ' ', 't', 'e', 'x', 't', ' ', 'p', 'r', 'o', 'c', 'e', 's', 's', 'i', 'n', 'g', ' ', 'w', 'h', 'e', 'r', 'e', ' ', 't', 'e', 'x', 't', ' ', 'i', 's', ' ', 's', 'p', 'l', 'i', 't', ' ', 'i', 'n', 't', 'o', ' ', 's', 'm', 'a', 'l', 'l', 'e', 'r', ' ', 'u', 'n', 'i', 't', 's', ',', ' ', 's', 'u', 'c', 'h', ' ', 'a', 's', ' ', 'w', 'o', 'r', 'd', 's', ',', ' ', 'p', 'h', 'r', 'a', 's', 'e', 's', ',', ' ', 'o', 'r', ' ', 's', 'y', 'm', 'b', 'o', 'l', 's', '.']
```
因為機器看不懂這些字符，所以我們要對他們進行編碼，將他們Mapping到一個數字上。
首先用`set()`把不重複的字符找出來，然後用`enumerate()`對他們進行編碼。

```python
map_dict = {k:v for v,k in enumerate(set(list(string)))}
print(map_dict)
```
可以得到輸出：
```
{'d': 0, 'r': 1, ',': 2, 'u': 3, 'T': 4, 'z': 5, 'p': 6, 'x': 7, 'c': 8, '.': 9, 'l': 10, ' ': 11, 'n': 12, 'h': 13, 's': 14, 'm': 15, 'y': 16, 'b': 17, 'i': 18, 'o': 19, 'g': 20, 'k': 21, 'a': 22, 'e': 23, 'w': 24, 't': 25}
```
最後再用`map()`函數對字符進行編碼。

```python
encoded_string = list(map(lambda x: map_dict[x], list(string)))
print(encoded_string)
```
可以得到輸出：
```
[2, 13, 4, 14, 24, 20, 25, 1, 10, 20, 13, 24, 12, 20, 16, 12, 1, 12, 0, 21, 8, 0, 20, 1, 5, 12, 16, 10, 14, 17, 12, 20, 24, 12, 10, 14, 15, 10, 12, 17, 21, 13, 0, 14, 16, 16, 20, 24, 19, 12, 18, 11, 14, 21, 14, 12, 10, 14, 15, 10, 12, 20, 16, 12, 16, 17, 5, 20, 10, 12, 20, 24, 10, 13, 12, 16, 22, 1, 5, 5, 14, 21, 12, 8, 24, 20, 10, 16, 23, 12, 16, 8, 0, 11, 12, 1, 16, 12, 18, 13, 21, 3, 16, 23, 12, 17, 11, 21, 1, 16, 14, 16, 23, 12, 13, 21, 12, 16, 6, 22, 9, 13, 5, 16, 7]
```

這些數字就是字符的編碼，機器可以通過這些數字來理解文本的內容，但是character tokenization的缺點非常明顯，就是完全他忽略了詞彙的意義，對於語言的理解是非常有限的。

### Word Tokenization

這邊示範一個最簡單的方法，就是使用Python的`split()`函數，將文本按照空格進行分割。
這個方法也叫做whitespace tokenization，它的缺點是無法處理標點符號，對於一些特殊的文本處理任務可能不夠靈活。

```python
string = 'Tokenization is a crucial step in text processing where text is split into smaller units, such as words, phrases, or symbols.'
print(string.split())
```

可以得到輸出：
```
['Tokenization', 'is', 'a', 'crucial', 'step', 'in', 'text', 'processing', 'where', 'text', 'is', 'split', 'into', 'smaller', 'units,', 'such', 'as', 'words,', 'phrases,', 'or', 'symbols.']
```
可以看到很多的標點符號都被當作單詞的一部分，這樣對於後續的處理會不太方便使用，因此我們需要一些其他更進階的演算法來進行tokenization。


### Subword Tokenization

在這個段落我會介紹一種最常見的Subword Tokenization方法 Byte Pair Encoding (BPE)。

#### Byte-Pair Encoding (BPE)

BPE的主要精神就是將相鄰兩個常見的字符組合成一個新的字符，然後不斷重複這個過程，直到達到指定的詞彙數量。步驟如下：

1. 初始化vocabulary list，將每個字符作為一個詞彙，`</w>`則是 end-of-word 的意思。
2. 將出現頻率最高的相鄰兩個字符組合成一個新的字符，並將這個字符加入vocabulary list。
3. 用新的vocabulary去取代原本的字符，重複步驟2直到達到指定的vocabulary數量。

以下是一個簡單的例子：
```python
vocab = {'l o w </w>' : 5,
         'l o w e r </w>' : 2,
         'n e w e s t </w>': 6,
         'w i d e s t </w>': 3
        }
```
一開始vocabulary list 裡面包含了所有的character：
```
</w> d e i l n o r s t w
```
一共有11個字符，此時相鄰的pair所對應倒的出現頻率如下： 

```
defaultdict(int,
            {('l', 'o'): 7,
             ('o', 'w'): 7,
             ('w', '</w>'): 5,
             ('w', 'e'): 8,
             ('e', 'r'): 2,
             ('r', '</w>'): 2,
             ('n', 'e'): 6,
             ('e', 'w'): 6,
             ('e', 's'): 9,
             ('s', 't'): 9,
             ('t', '</w>'): 9,
             ('w', 'i'): 3,
             ('i', 'd'): 3,
             ('d', 'e'): 3})
```

出現頻率最高的相鄰兩個字符有三種，其中一種是`e s`，頻率為 9，所以我們將`e s`組合成一個新的字符`es`，然後加入vocabulary list：
```
</w> d e i l n o r s t w es
```
此時再計算相鄰的pair所對應倒的出現頻率：
```python
defaultdict(int,
            {('l', 'o'): 7,
             ('o', 'w'): 7,
             ('w', '</w>'): 5,
             ('w', 'e'): 2,
             ('e', 'r'): 2,
             ('r', '</w>'): 2,
             ('n', 'e'): 6,
             ('e', 'w'): 6,
             ('w', 'es'): 6,
             ('es', 't'): 9,
             ('t', '</w>'): 9,
             ('w', 'i'): 3,
             ('i', 'd'): 3,
             ('d', 'es'): 3})
```
可以看到`es t`的頻率最高，所以我們將`es t`組合成一個新的字符`est`，然後加入vocabulary list：
```
</w> d e i l n o r s t w es est
```
接著我們一直重複以上步驟，直到達到指定的vocabulary數量為止，假設數量為16，那麼最後的vocabulary list如下：
```
</w> d e i l n o r s t w es est est</w> lo low
```
這樣我們就得到了一個包含16個詞彙的vocabulary list，這個list可以用來將文本轉換為subwords。  
比如說我們有一個句子`lowest`，我們可以用這個vocabulary list將它轉換為`low est</w>`。
若是遇到一個不在vocabulary list裡面的詞彙，我們可以用`<unk>`來表示。  
比如說我們有一個句子`fest`，我們可以用這個vocabulary list將它轉換為`<unk> est</w>`。

## References


---

本文是我的第二篇文章，如果您喜歡，請繼續關注我的Blog /w