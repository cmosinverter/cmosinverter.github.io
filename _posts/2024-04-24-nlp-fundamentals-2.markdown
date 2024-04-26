---
layout:     post
title:      "NLP基礎學習(二) - Text Preprocessing with spaCy"
subtitle:   "文本預處理的基本概念"
date:       2024-04-25
author:     "PCLiu"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
tags:
  - NLP
---


> 在本文中，我們將探討 Spacy 函式庫的基本概念，以及如何使用 Spacy 進行文本預處理。Spacy 是一個用於自然語言處理的Python函式庫，它提供了許多功能，包括詞性標註、命名實體識別、句法分析等。在本文中，我們將使用 Spacy 來進行文本預處理，包括分詞、詞性標註、命名實體識別等。


### Installation of spaCy

先安裝spaCy函式庫，可以使用以下命令：

```
pip install spacy
```

安裝完成後，我們需要下載spaCy的模型，可以使用以下命令：

```
python -m spacy download en_core_web_sm
```

### The Doc Object for Processed Text

我們首先創建一個叫做`nlp`的spaCy物件，這個物件幾乎包含了spaCy的所有功能。然後我們使用這個物件來處理文本，得到一個叫做`doc`的物件，這個物件包含了處理後的文本。`en_core_web_sm`則是spaCy的一個模型，它包含了英文的詞彙表和語法規則，如果你想要處理其他語言，可以到[spaCy官網](https://spacy.io/models)下載相應的模型。

```python
nlp = spacy.load("en_core_web_sm")
introduction_doc = nlp("In 1991, the World Wide Web was born. It was a medium for sharing information.")
print([token.text for token in introduction_doc])
```
此時輸出的結果是：

```
['In', '1991', ',', 'the', 'World', 'Wide', 'Web', 'was', 'born', '.', 'It', 'was', 'a', 'medium', 'for', 'sharing', 'information', '.']
```
這裡scapy已經幫我們處將文本的內容切成了token的形式，我們可以通過`token.text`來獲取token的內容。

### The Doc Object for Processed Text (Pass a .txt file)

用同樣的方式以可以處理`.txt`文件：

```python
file_name = "introduction.txt"
introduction_doc = nlp(pathlib.Path(file_name).read_text(encoding="utf-8"))
print([token.text for token in introduction_doc])
```

### Sentence Detection

spaCy還可以幫我們檢測句子，我們可以通過`doc.sents`來獲取句子：

```python
about_text = (
      "Gus Proto is a Python developer currently"
      " working for a London-based Fintech"
      " company. He is interested in learning"
      " Natural Language Processing."
)
about_doc = nlp(about_text)
sentences = list(about_doc.sents)
print(sentences)
```

輸出結果：

```
[Gus Proto is a Python developer currently working for a London-based Fintech company.,  
 He is interested in learning Natural Language Processing.]
```
### Tokens in spaCy

透過`token.idx`可以獲取token在文本中的位置：

```python
print(f"{"Token":12} {"Start Index"}")
for token in about_doc:
    print(f"{token.text:12} {token.idx}")
```

輸出結果：

```
Token        Start Index
Gus          0
Proto        4
is           10
a            13
Python       15
developer    22
currently    32
working      42
for          50
a            54
London       56
-            62
based        63
Fintech      69
company      77
.            84
He           86
is           89
interested   92
in           103
learning     106
Natural      115
Language     123
Processing   132
.            142
```

spaCy還可以幫我們檢測token是否為英文字母所組成，是否為標點符號，是否為`Stop Word`等：  
我會在下文中介紹`Stop Words`。

```python
print(
f"{"Text with Whitespace":25}"
f"{"Is Alphanumeric?":20}"
f"{"Is Punctuation?":20}"
f"{"Is Stop Word?"}"
)

for token in about_doc:
    print(
         f"{str(token.text_with_ws):25}"
         f"{str(token.is_alpha):20}"
         f"{str(token.is_punct):20}"
         f"{str(token.is_stop)}"
    )
```

輸出結果：

```
Text with Whitespace     Is Alphanumeric?    Is Punctuation?     Is Stop Word?
Gus                      True                False               False
Proto                    True                False               False
is                       True                False               True
a                        True                False               True
Python                   True                False               False
developer                True                False               False
currently                True                False               False
working                  True                False               False
for                      True                False               True
a                        True                False               True
London                   True                False               False
-                        False               True                False
based                    True                False               False
Fintech                  True                False               False
company                  True                False               False
.                        False               True                False
He                       True                False               True
is                       True                False               True
interested               True                False               False
in                       True                False               True
learning                 True                False               False
Natural                  True                False               False
Language                 True                False               False
Processing               True                False               False
.                        False               True                False
```

### Stop Words

`Stop Words`是一些在文本處理中通常會被忽略的詞彙(且忽略之後對原本的語義沒有太大影響)，比如`is`、`the`、`a`等。spaCy提供了一個`STOP_WORDS`的集合，我們可以通過這個集合來檢測token是否為`Stop Word`。

這裡我們列出前10個`Stop Words`：
```python
spacy_stopwords = spacy.lang.en.stop_words.STOP_WORDS
for stop_word in list(spacy_stopwords)[:10]:
    print(stop_word)
```
輸出結果：

```
made
we
will
nothing
afterwards
thru
alone
much
side
's
```
幾乎都是一些常見的英文`Stop Words`。

透過`token.is_stop`可以檢測token是否為`Stop Word`，並且根據這個屬性來過濾掉`Stop Words`：

```python
custom_about_text = (
     "Gus Proto is a Python developer currently"
     " working for a London-based Fintech"
     " company. He is interested in learning"
     " Natural Language Processing."
 )

nlp = spacy.load("en_core_web_sm")
about_doc = nlp(custom_about_text)
print([token for token in about_doc if not token.is_stop])
```

輸出結果：

```
[Gus, Proto, Python, developer, currently, working, London, -, based, Fintech, company, ., interested, learning, Natural, Language, Processing, .]
```
可以看到`is`、`a`等`Stop Words`都被過濾掉了。

### Lemmatization

`Lemmatization`是將單詞還原為它的基本形式的過程，比如`running`還原為`run`。spaCy提供了一個`lemma_`屬性，我們可以通過這個屬性來獲取token的基本形式。

```python
conference_help_text = (
     "Gus is helping organize a developer"
     " conference on Applications of Natural Language"
     " Processing. He keeps organizing local Python meetups"
     " and several internal talks at his workplace."
 )

nlp = spacy.load("en_core_web_sm")
conference_help_doc = nlp(conference_help_text)

for token in conference_help_doc:
    if str(token) != str(token.lemma_): # only print if lemma is different with the original token
        print(f"{str(token):>20} : {str(token.lemma_)}")
```

輸出結果：

```
                  is : be
                  He : he
               keeps : keep
          organizing : organize
             meetups : meetup
               talks : talk
```

可以看到`is`被還原為`be`，`keeps`被還原為`keep`等。

### Word Frequency

我們可以通過`Counter`來計算文本中每個單詞的出現次數，在計算之前我們需要過濾掉`Stop Words`和標點符號：

```python
from collections import Counter
nlp = spacy.load("en_core_web_sm")
complete_text = (
     "Gus Proto is a Python developer currently"
     " working for a London-based Fintech company. He is"
     " interested in learning Natural Language Processing."
     " There is a developer conference happening on 21 July"
     ' 2019 in London. It is titled "Applications of Natural'
     ' Language Processing". There is a helpline number'
     " available at +44-1234567891. Gus is helping organize it."
     " He keeps organizing local Python meetups and several"
     " internal talks at his workplace. Gus is also presenting"
     ' a talk. The talk will introduce the reader about "Use'
     ' cases of Natural Language Processing in Fintech".'
     " Apart from his work, he is very passionate about music."
     " Gus is learning to play the Piano. He has enrolled"
     " himself in the weekend batch of Great Piano Academy."
     " Great Piano Academy is situated in Mayfair or the City"
     " of London and has world-class piano instructors."
 )
complete_doc = nlp(complete_text)

words = [
     token.text
     for token in complete_doc
     if not token.is_stop and not token.is_punct # filter out stop words and punctuation
]

print(Counter(words).most_common(5))
```

輸出結果：

```
[('Gus', 4), ('London', 3), ('Natural', 3), ('Language', 3), ('Processing', 3)]
```

可以看到`Gus`出現了4次，`London`、`Natural`、`Language`和`Processing`都出現了3次。  
光是從這個結果就可以看出這段文本的主題是關於`Gus`、`London`和`Natural Language Processing`。

### Part-of-Speech Tagging

POS標註是將文本中的每個token標註為詞性的過程，比如名詞、動詞、形容詞等。spaCy提供了一個`pos_`屬性，我們可以通過這個屬性來獲取token的詞性。  

下面是8種常見的屬性:

```
Noun
Pronoun
Adjective
Verb
Adverb
Preposition
Conjunction
Interjection
```

下面是一個簡單的例子：
```python
about_text = (
     "Gus Proto is a Python developer currently"
     " working for a London-based Fintech"
     " company. He is interested in learning"
     " Natural Language Processing."
 )

about_doc = nlp(about_text)

for token in about_doc:
    print(
        f"""
        TOKEN: {str(token)}
        =====
        POS: {token.pos_}
        EXPLANATION: {spacy.explain(token.tag_)}"""
        )
```

輸出結果：

```
        TOKEN: Gus
        =====
        POS: PROPN
        EXPLANATION: noun, proper singular

        TOKEN: Proto
        =====
        POS: PROPN
        EXPLANATION: noun, proper singular

        TOKEN: is
        =====
        POS: AUX
        EXPLANATION: verb, 3rd person singular present

        TOKEN: a
        =====
        POS: DET
        EXPLANATION: determiner

        TOKEN: Python
        =====
        POS: PROPN
        EXPLANATION: noun, proper singular
```

### Processing functions

下面是一個使用spaCy進行文本預處理的例子，包括分詞、過濾掉`Stop Words`和標點符號、還原詞形等等。

```python
nlp = spacy.load("en_core_web_sm")
complete_text = (
     "Gus Proto is a Python developer currently"
     " working for a London-based Fintech company. He is"
     " interested in learning Natural Language Processing."
     " There is a developer conference happening on 21 July"
     ' 2019 in London. It is titled "Applications of Natural'
     ' Language Processing". There is a helpline number'
     " available at +44-1234567891. Gus is helping organize it."
     " He keeps organizing local Python meetups and several"
     " internal talks at his workplace. Gus is also presenting"
     ' a talk. The talk will introduce the reader about "Use'
     ' cases of Natural Language Processing in Fintech".'
     " Apart from his work, he is very passionate about music."
     " Gus is learning to play the Piano. He has enrolled"
     " himself in the weekend batch of Great Piano Academy."
     " Great Piano Academy is situated in Mayfair or the City"
     " of London and has world-class piano instructors."
 )

complete_doc = nlp(complete_text)

def is_token_allowed(token):
    return bool( token and str(token).strip() and not token.is_stop and not token.is_punct )

def preprocess_token(token):
    return token.lemma_.strip().lower()

complete_filtered_tokens = [
     preprocess_token(token)
     for token in complete_doc
     if is_token_allowed(token)
]

print(complete_filtered_tokens)
```

輸出結果：
```
['gus', 'proto', 'python', 'developer', 'currently', 'work', 'london', 'base', 'fintech', 'company', 'interested', 'learn', 'natural', 'language', 'processing', 'developer', 'conference', 'happen', '21', 'july', '2019', 'london', 'title', 'application', 'natural', 'language', 'processing', 'helpline', 'number', 'available', '+44', '1234567891', 'gus', 'helping', 'organize', 'keep', 'organize', 'local', 'python', 'meetup', 'internal', 'talk', 'workplace', 'gus', 'present', 'talk', 'talk', 'introduce', 'reader', 'use', 'case', 'natural', 'language', 'processing', 'fintech', 'apart', 'work', 'passionate', 'music', 'gus', 'learn', 'play', 'piano', 'enrol', 'weekend', 'batch', 'great', 'piano', 'academy', 'great', 'piano', 'academy', 'situate', 'mayfair', 'city', 'london', 'world', 'class', 'piano', 'instructor']
```

### Rule-based Matching

Rule-based Matching是一種通過定義規則來匹配文本的方法，spaCy提供了一個`Matcher`類，我們可以通過這個類來定義匹配規則。下面是一個簡單的例子：  

假如說我們想要將`Gus Proto` match 成一個叫做`FULL_NAME`的pattern，我們可以這樣做：  
因為`Gus`和`Proto`都是`PROPN`，所以我們可以通過這個特性來定義我們的`pattern`。

```python
from spacy.matcher import Matcher

nlp = spacy.load("en_core_web_sm")

about_text = (
     "Gus Proto is a Python developer currently"
     " working for a London-based Fintech"
     " company. He is interested in learning"
     " Natural Language Processing."
 )

about_doc = nlp(about_text)

matcher = Matcher(nlp.vocab)

def extract_full_name(nlp_doc):
    
    name = []
    pattern = [{"POS": "PROPN"}, {"POS": "PROPN"}] # PROPN: proper noun, 連續兩個PROPN
    matcher.add("FULL_NAME", [pattern])
    matches = matcher(nlp_doc)
    for _, start, end in matches:
        span = nlp_doc[start:end]
        name.append(span.text)
    
    return name
        
extract_full_name(about_doc)
```

輸出結果：

```
['Gus Proto', 'Natural Language', 'Language Processing']
```
可以看到`Gus Proto`，`Natural Language`和`Language Processing`都被成功匹配了，因為他們都是由兩個`PROPN`組成的。  
可是還是有一些問題，比如`Natural Language`和`Language Processing`被匹配了兩次，這是因為`Natural`和`Language`都是`PROPN`，所以他們也被匹配了。  
所以如果只是想要匹配`Gus Proto`，我們可以利用接下來要介紹的`Named Entity Recognition`。

### Named Entity Recognition

命名實體識別是將文本中的命名實體識別出來的過程，比如人名、地名、組織名等。spaCy提供了一個`ent_type_`屬性，我們可以通過這個屬性來獲取token的命名實體類型。

透過`doc.ents`可以獲取文本中的所有命名實體：
```python
nlp = spacy.load("en_core_web_sm")

piano_class_text = (
     "Great Piano Academy is situated"
     " in Mayfair or the City of London and has"
     " world-class piano instructors."
 )

piano_class_doc = nlp(piano_class_text)
for ent in piano_class_doc.ents:
    print(
        f"""
        {ent.text = }
        {ent.start_char = }
        {ent.end_char = }
        {ent.label_ = }
        spacy.explain('{ent.label_}') = {spacy.explain(ent.label_)}"""
        )
```

輸出結果：

```

        ent.text = 'Great Piano Academy'
        ent.start_char = 0
        ent.end_char = 19
        ent.label_ = 'ORG'
        spacy.explain('ORG') = Companies, agencies, institutions, etc.

        ent.text = 'Mayfair'
        ent.start_char = 35
        ent.end_char = 42
        ent.label_ = 'GPE'
        spacy.explain('GPE') = Countries, cities, states

        ent.text = 'the City of London'
        ent.start_char = 46
        ent.end_char = 64
        ent.label_ = 'GPE'
        spacy.explain('GPE') = Countries, cities, states
```

可以看到`Great Piano Academy`被識別為`ORG`，`Mayfair`和`the City of London`被識別為`GPE`。  
其中`ORG`代表`Companies, agencies, institutions, etc.`，`GPE`代表`Countries, cities, states`，可以用`spacy.explain()`來獲取這些標籤的解釋。


以下是一個簡單的例子，讓我們可以便釋出文本中的人名，然後將他們屏蔽掉：

```python
survey_text = (
    "Out of 5 people surveyed, James Robert,"
    " Julie Fuller and Benjamin Brooks like"
    " apples. Kelly Cox and Matthew Evans"
    " like oranges."
)

def replace_person_names(token):
    if token.ent_iob != 0 and token.ent_type_ == "PERSON":
        return "[REDACTED] "
    return token.text_with_ws

def redact_names(nlp_doc):
    with nlp_doc.retokenize() as retokenizer:
        for ent in nlp_doc.ents:
            retokenizer.merge(ent)
    tokens = map(replace_person_names, nlp_doc)
    return "".join(tokens)

survey_doc = nlp(survey_text)
print(redact_names(survey_doc))
```

輸出結果：

```
Out of 5 people surveyed, [REDACTED] , [REDACTED] and [REDACTED] like apples. [REDACTED] and [REDACTED] like oranges.
```

可以看到`James Robert`、`Julie Fuller`、`Benjamin Brooks`、`Kelly Cox`、`Matthew Evans`都被屏蔽掉了。


### References

[1] [Natural Language Processing With spaCy in Python by Taranjeet Singh](https://realpython.com/natural-language-processing-spacy-python/#introduction-to-nlp-and-spacy)  
[2] [spaCy 官方網站](https://spacy.io/)

如果您喜歡本篇文章，請繼續關注我的Blog :)