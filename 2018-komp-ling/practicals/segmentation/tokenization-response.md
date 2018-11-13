## Отчет по токенизации

В работе я использовал следующую реализацию maxmatch:

```
# max_match
# sentence – предложение, которое предстоит токенизировать
# wordlist - список уникальных словоформ, по которым происходит поиск токенов
def max_match(sentence, wordlist):
    if not sentence:
        return []
    for i in range(len(sentence)-1, -1, -1):
        first_word = (sentence[0:i+1])
        remainder = sentence[i+1:len(sentence)]
        if first_word in wordlist:
            return [first_word] + max_match(remainder, wordlist)

    # если слово не найдено, то создаем новое односимвольное слово
    first_word = sentence[0]
    remainder = sentence[1:len(sentence)]

    return [first_word] + max_match(remainder, wordlist)
```

Для алгоритма необходимо предварительно подготовить список уникальных словоформ (wordlist), по которым затем будет происходить поиск токенов в предложениях. На вход алгоритму подаются предложения для токенизации и подготовленный список словоформ (wordlist).

[Подробный код исследования](Tokenization.md)


### Пример работы алгоритма для тестового набора данных из UD_Japanese-GSD:

Исходное предложение (sentence):

> 幸福の科学側からは,特にどうしてほしいという要望はいただいていません。

Список токенов исходного предложения (reference_sentence):

> '幸福', 'の', '科学', '側', 'から', 'は', ',', '特に', 'どうして', 'ほしい', 'という', '要望', 'は', 'いただい', 'て', 'い', 'ませ', 'ん', '。'

Список токенов, полученных после обработки исходного предложения алгоритмом maxmatch (hypothesis_sentence):

> '幸福', 'の', '科学', '側', 'から', 'は', ',', '特に', 'どうして', 'ほしい', 'という', '要望', 'は', 'いただい', 'て', 'いま', 'せ', 'ん', '。'

**Алгоритм ошибся при токенизации двух слов: 'い', 'ませ'.**


### Качество токенизации

Алгоритм maxmatch довольно требовательный к ресурсам, поэтому в исследовании я ограничился токенизацией первых ста предложений.

**Медианное значение ошибки (WER) для первых ста предложений из тестового набора равно 21,85%.**

