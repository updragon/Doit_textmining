Quiz part 01
============

### `speech_park.txt`에는 박근혜 전 대통령의 대선 출마 선언문이 들어있습니다. `speech_park.txt`를 이용해 문제를 해결해 보세요.

#### Q1. `speech_park.txt`를 불러와 분석에 적합하게 전처리한 다음 띄어쓰기 기준으로 토큰화하세요.

##### (1) 전처리

``` r
raw_park <- readLines("speech_park.txt", encoding = "UTF-8")
```

``` r
library(dplyr)
library(stringr)
park <- raw_park %>%
  str_replace_all("[^가-힣]", " ") %>%  # 한글만 남기기
  str_squish() %>%                      # 연속된 공백 제거
  as_tibble()                           # tibble로 변환

park
```

    ## # A tibble: 96 x 1
    ##    value                                                                        
    ##    <chr>                                                                        
    ##  1 "존경하는 국민 여러분 저는 오늘 국민 한 분 한 분의 꿈이 이루어지는 행복한 대한민국을 만들기 위해 저의 모든 것을 바치겠다는 각오로 ~
    ##  2 ""                                                                           
    ##  3 "국민 여러분 저의 삶은 대한민국과 함께 해온 시간이었습니다 우리나라가 가난을 이기고 꿈을 이뤄가는 위대한 과정을 어린 시절부터 가슴깊이~
    ##  4 ""                                                                           
    ##  5 "어머니가 흉탄에 돌아가신 후 견딜 수 없는 고통과 어려움 속에서도 그 힘든 시간을 이겨낼 수 있었던 것은 어머니의 빈자리에 대한 책임감~
    ##  6 ""                                                                           
    ##  7 "그때부터 제 삶은 완전히 다른 길을 가야했습니다 개인의 삶 대신 국민과 함께 하는 공적인 삶이 시작되었습니다 각계각층의 국민들을 만나고~
    ##  8 ""                                                                           
    ##  9 "아버지를 잃는 또 다른 고통과 아픔을 겪고 저는 평범한 삶을 살고자 했습니다 하지만 국민들의 땀과 눈물로 이룩해 온 나라가 외환위기를 ~
    ## 10 ""                                                                           
    ## # ... with 86 more rows

##### (2) 토큰화

``` r
library(tidytext)
word_space <- park %>%
  unnest_tokens(input = value,
                output = word,
                token = "words")        # 띄어쓰기 기준

word_space
```

    ## # A tibble: 1,414 x 1
    ##    word    
    ##    <chr>   
    ##  1 존경하는
    ##  2 국민    
    ##  3 여러분  
    ##  4 저는    
    ##  5 오늘    
    ##  6 국민    
    ##  7 한      
    ##  8 분      
    ##  9 한      
    ## 10 분의    
    ## # ... with 1,404 more rows

##### Q2. 가장 자주 사용된 단어 20개를 추출하세요.

``` r
top20 <- word_space %>%
  count(word, sort = T) %>%
  filter(str_count(word) > 1) %>%
  head(20)

top20
```

    ## # A tibble: 20 x 2
    ##    word             n
    ##    <chr>        <int>
    ##  1 국민            29
    ##  2 저는            14
    ##  3 있습니다        12
    ##  4 함께            12
    ##  5 꿈을            10
    ##  6 것입니다         8
    ##  7 새로운           8
    ##  8 있는             8
    ##  9 국민행복의       7
    ## 10 길을             7
    ## 11 것이             6
    ## 12 국민들의         6
    ## 13 만들겠습니다     6
    ## 14 박근혜           6
    ## 15 아니라           6
    ## 16 여러분의         6
    ## 17 우리             6
    ## 18 있도록           6
    ## 19 통해             6
    ## 20 대한             5

##### Q3. 가장 자주 사용된 단어 20개의 빈도를 나타낸 막대 그래프를 만드세요. 그래프의 폰트는 나눔고딕으로 설정하세요.

##### (1) 폰트 설정

``` r
library(showtext)
font_add_google(name = "Nanum Gothic", family = "nanumgothic")
showtext_auto()
```

##### (2) 막대 그래프 만들기

``` r
library(ggplot2)
ggplot(top20, aes(x = reorder(word, n), y = n)) +
  geom_col() +
  coord_flip () +
  geom_text(aes(label = n), hjust = -0.3) +
  labs(x = NULL) +
  theme(text = element_text(family = "nanumgothic"))
```

![](Quiz_files/figure-markdown_github/unnamed-chunk-8-1.png)

<!-- # 2장 -->
<!-- #### 박근혜 전 대통령의 대선 출마 선언문이 들어있는 `speech_park.txt`를 이용해 문제를 해결해 보세요. -->
<!-- ##### Q1. `speech_park.txt`를 불러와 분석에 적합하게 전처리한 다음 연설문에서 명사를 추출하세요. -->
<!-- ```{r eval=F} -->
<!-- raw_park <- readLines("speech_park.txt", encoding = "UTF-8") -->
<!-- ``` -->
<!-- ```{r echo=F} -->
<!-- raw_park <- readLines(here("files/speech_park.txt"), encoding = "UTF-8") -->
<!-- ``` -->
<!-- ```{r} -->
<!-- # 전처리 -->
<!-- library(dplyr) -->
<!-- library(stringr) -->
<!-- park <- raw_park %>% -->
<!--   str_replace_all("[^가-힣]", " ") %>%  # 한글만 남기기 -->
<!--   str_squish() %>%                      # 연속된 공백 제거 -->
<!--   as_tibble()                           # tibble로 변환 -->
<!-- park -->
<!-- # 명사 기준 토큰화 -->
<!-- library(tidytext) -->
<!-- library(KoNLP) -->
<!-- word_noun <- park %>% -->
<!--   unnest_tokens(input = value, -->
<!--                 output = word, -->
<!--                 token = extractNoun) -->
<!-- word_noun -->
<!-- ``` -->
<!-- ##### Q2. 가장 자주 사용된 단어 20개를 추출하세요. -->
<!-- ```{r} -->
<!-- top20 <- word_noun %>% -->
<!--   count(word, sort = T) %>% -->
<!--   filter(str_count(word) > 1) %>% -->
<!--   head(20) -->
<!-- top20 -->
<!-- ``` -->
<!-- ##### Q3. 가장 자주 사용된 단어 20개의 빈도를 나타낸 막대 그래프를 만드세요. -->
<!-- ```{r} -->
<!-- library(ggplot2) -->
<!-- ggplot(top20, aes(x = reorder(word, n), y = n)) + -->
<!--   geom_col() + -->
<!--   coord_flip () + -->
<!--   geom_text(aes(label = n), hjust = -0.3) + -->
<!--   labs(x = NULL) -->
<!-- ``` -->
<!-- ##### Q4. 전처리하지 않은 연설문에서 연속된 공백을 제거하고 tibble 구조로 변환한 다음 문장 기준으로 토큰화하세요. -->
<!-- ```{r} -->
<!-- sentences_park <- raw_park %>% -->
<!--   str_squish() %>%                    # 연속된 공백 제거 -->
<!--   as_tibble() %>%                     # tibble로 변환 -->
<!--   unnest_tokens(input = value, -->
<!--                 output = sentence, -->
<!--                 token = "sentences") -->
<!-- sentences_park -->
<!-- ``` -->
<!-- ##### Q5. 연설문에서 `"경제"`가 사용된 문장을 출력하세요. -->
<!-- ```{r} -->
<!-- sentences_park %>% -->
<!--   filter(str_detect(sentence, "경제")) -->
<!-- ``` -->
<!-- # 3장 -->
<!-- 역대 대통령의 대선 출마 선언문을 담은 `speeches_presidents.csv`를 이용해 문제를 해결해 보세요. -->
<!-- Q1.1 다음 코드를 실행해 `speeches_presidents.csv`를 불러온 다음 이명박 전 대통령과 노무현 전 대통령의 연설문을 추출하고 분석에 적합하게 전처리하세요. -->
<!-- ```{r eval=F, echo=T} -->
<!-- install.packages("readr") -->
<!-- library(readr) -->
<!-- raw_speeches <- read_csv("speeches_presidents.csv") -->
<!-- ``` -->
<!-- ```{r echo=F} -->
<!-- library(readr) -->
<!-- raw_speeches <- read_csv(here("files/speeches_presidents.csv")) -->
<!-- ``` -->
<!-- - [꿀팁] `read_csv()`는 데이터를 다루기 편한 tibble 구조로 만들어 주고 데이터를 불러오는 속도도 `read.csv()`보다 빠릅니다. -->
<!-- ```{r} -->
<!-- library(dplyr) -->
<!-- library(stringr) -->
<!-- speeches <- raw_speeches %>% -->
<!--   filter(president %in% c("이명박", "노무현")) %>% -->
<!--   mutate(value = str_replace_all(value, "[^가-힣]", " "), -->
<!--          value = str_squish(value)) -->
<!-- speeches -->
<!-- ``` -->
<!-- Q1.2 연설문에서 명사를 추출한 다음 연설문별 단어 빈도를 구하세요. -->
<!-- ```{r} -->
<!-- # 명사 추출 -->
<!-- library(tidytext) -->
<!-- library(KoNLP) -->
<!-- speeches <- speeches %>% -->
<!--   unnest_tokens(input = value, -->
<!--                 output = word, -->
<!--                 token = extractNoun) -->
<!-- speeches -->
<!-- # 연설문별 단어 빈도 구하기 -->
<!-- frequency <- speeches %>% -->
<!--   count(president, word) %>% -->
<!--   filter(str_count(word) > 1) -->
<!-- frequency -->
<!-- ``` -->
<!-- Q1.3 로그 오즈비를 이용해 두 연설문에서 상대적으로 중요한 단어를 10개씩 추출하세요. -->
<!-- ```{r} -->
<!-- # long form을 wide form으로 변환 -->
<!-- library(tidyr) -->
<!-- frequency_wide <- frequency %>% -->
<!--   pivot_wider(names_from = president,     # 변수명으로 만들 값 -->
<!--               values_from = n,            # 변수에 채워 넣을 값 -->
<!--               values_fill = list(n = 0))  # 결측치 0으로 변환 -->
<!-- frequency_wide -->
<!-- # 로그 오즈비 구하기 -->
<!-- frequency_wide <- frequency_wide %>% -->
<!--   mutate(log_odds_ratio = log(((이명박 + 1) / (sum(이명박 + 1))) / -->
<!--                               ((노무현 + 1) / (sum(노무현 + 1))))) -->
<!-- frequency_wide -->
<!-- # 상대적으로 중요한 단어 추출 -->
<!-- top10 <- frequency_wide %>% -->
<!--   group_by(president = ifelse(log_odds_ratio > 0, "lee", "roh")) %>% -->
<!--   slice_max(abs(log_odds_ratio), n = 10, with_ties = F) -->
<!-- top10 -->
<!-- ``` -->
<!-- Q1.4 두 연설문에서 상대적으로 중요한 단어를 나타낸 막대 그래프를 만드세요. -->
<!-- ```{r} -->
<!-- library(ggplot2) -->
<!-- ggplot(top10, aes(x = reorder(word, log_odds_ratio), -->
<!--                   y = log_odds_ratio, -->
<!--                   fill = president)) + -->
<!--   geom_col() + -->
<!--   coord_flip () + -->
<!--   labs(x = NULL) -->
<!-- ``` -->
<!-- <br> -->
<!-- <br> -->
<!-- 역대 대통령의 취임사를 담은 `inaugural_address.csv`를 이용해 문제를 해결해 보세요. -->
<!-- Q2.1 다음 코드를 실행해 `inaugural_address.csv`를 불러와 분석에 적합하게 전처리한 다음 연설문에서 명사를 추출하세요. -->
<!-- ```{r echo=T} -->
<!-- raw_speeches <- read_csv("inaugural_address.csv") -->
<!-- ``` -->
<!-- ```{r echo=F} -->
<!-- library(readr) -->
<!-- raw_speeches <- read_csv(here("files/inaugural_address.csv")) -->
<!-- ``` -->
<!-- ```{r} -->
<!-- # 기본적인 전처리 -->
<!-- library(dplyr) -->
<!-- library(stringr) -->
<!-- speeches <- raw_speeches %>% -->
<!--   mutate(value = str_replace_all(value, "[^가-힣]", " "), -->
<!--          value = str_squish(value)) -->
<!-- speeches -->
<!-- # 명사 기준 토큰화 -->
<!-- library(tidytext) -->
<!-- library(KoNLP) -->
<!-- speeches <- speeches %>% -->
<!--   unnest_tokens(input = value, -->
<!--                 output = word, -->
<!--                 token = extractNoun) -->
<!-- speeches -->
<!-- ``` -->
<!-- - [꿀팁] 문재인 대통령의 취임사 출처: bit.ly/easytext_34 -->
<!-- - [꿀팁] 이명박, 박근혜, 노무현 전 대통령의 취임사 출처: bit.ly/easytext_35 -->
<!-- Q2.2 TF-IDF를 이용해 각 연설문에서 상대적으로 중요한 단어를 10개씩 추출하세요. -->
<!-- ```{r} -->
<!-- # 단어 빈도 구하기 -->
<!-- frequecy <- speeches %>% -->
<!--   count(president, word) %>% -->
<!--   filter(str_count(word) > 1) -->
<!-- frequecy -->
<!-- # TF-IDF 구하기 -->
<!-- frequecy <- frequecy %>% -->
<!--   bind_tf_idf(term = word,           # 단어 -->
<!--               document = president,  # 텍스트 구분 변수 -->
<!--               n = n) %>%             # 단어 빈도 -->
<!--   arrange(-tf_idf) -->
<!-- frequecy -->
<!-- # 상대적으로 중요한 단어 추출 -->
<!-- top10 <- frequecy %>% -->
<!--   group_by(president) %>% -->
<!--   slice_max(tf_idf, n = 10, with_ties = F) -->
<!-- top10 -->
<!-- ``` -->
<!-- Q2.3 각 연설문에서 상대적으로 중요한 단어를 나타낸 막대 그래프를 만드세요. -->
<!-- ```{r} -->
<!-- library(ggplot2) -->
<!-- ggplot(top10, aes(x = reorder_within(word, tf_idf, president), -->
<!--                   y = tf_idf, -->
<!--                   fill = president)) + -->
<!--   geom_col(show.legend = F) + -->
<!--   coord_flip () + -->
<!--   facet_wrap(~ president, scales = "free", ncol = 2) + -->
<!--   scale_x_reordered() + -->
<!--   labs(x = NULL) -->
<!-- ``` -->
<!-- # 4장 -->
<!-- `"news_comment_BTS.csv"`에는 2020년 9월 21일 방탄소년단이 '빌보드 핫 100 차트' 1위에 오른 소식을 다룬 기사에 달린 댓글이 들어있습니다. `"news_comment_BTS.csv"`를 이용해 문제를 해결해 보세요. -->
<!-- Q1. `"news_comment_BTS.csv"`를 불러온 다음 행 번호를 나타낸 변수를 추가하고 분석에 적합하게 전처리하세요. -->
<!-- ```{r eval=F} -->
<!-- # 기사 댓글 불러오기 -->
<!-- library(readr) -->
<!-- library(dplyr) -->
<!-- raw_news_comment <- read_csv("news_comment_BTS.csv") -->
<!-- glimpse(raw_news_comment) -->
<!-- ``` -->
<!-- ```{r echo=F} -->
<!-- library(readr) -->
<!-- library(dplyr) -->
<!-- raw_news_comment <- read_csv(here::here("files/news_comment_BTS.csv")) -->
<!-- glimpse(raw_news_comment) -->
<!-- ``` -->
<!-- ```{r} -->
<!-- # 전처리 -->
<!-- library(stringr) -->
<!-- library(textclean) -->
<!-- news_comment <- raw_news_comment %>% -->
<!--   mutate(id = row_number(), -->
<!--          reply = str_squish(replace_html(reply))) -->
<!-- news_comment %>% -->
<!--   select(id, reply) -->
<!-- ``` -->
<!-- Q2. 댓글을 띄어쓰기 기준으로 토큰화하고 감정 사전을 이용해 댓글의 감정 점수를 구하세요. -->
<!-- ```{r} -->
<!-- # 토큰화 -->
<!-- library(tidytext) -->
<!-- library(KoNLP) -->
<!-- word_comment <- news_comment %>% -->
<!--   unnest_tokens(input = reply, -->
<!--                 output = word, -->
<!--                 token = "words",  # 띄어쓰기 기준 -->
<!--                 drop = F)         # 원문 유지 -->
<!-- word_comment %>% -->
<!--   select(word) -->
<!-- ``` -->
<!-- ```{r eval=F} -->
<!-- # 감정 사전 불러오기 -->
<!-- dic <- read_csv("knu_sentiment_lexicon.csv") -->
<!-- ``` -->
<!-- ```{r echo=F} -->
<!-- # 감정 사전 불러오기 -->
<!-- dic <- read_csv(here("files/knu_sentiment_lexicon.csv")) -->
<!-- ``` -->
<!-- ```{r} -->
<!-- # 단어에 감정 점수 부여 -->
<!-- word_comment <- word_comment %>% -->
<!--   left_join(dic, by = "word") %>% -->
<!--   mutate(polarity = ifelse(is.na(polarity), 0, polarity)) -->
<!-- word_comment %>% -->
<!--   select(word, polarity) %>% -->
<!--   arrange(-polarity) -->
<!-- # 댓글별로 단어의 감정 점수 합산 -->
<!-- score_comment <- word_comment %>% -->
<!--   group_by(id, reply) %>% -->
<!--   summarise(score = sum(polarity)) %>% -->
<!--   ungroup() -->
<!-- score_comment %>% -->
<!--   select(score, reply) %>% -->
<!--   arrange(-score) -->
<!-- ``` -->
<!-- Q3. 감정 범주별 댓글 빈도를 나타낸 막대 그래프를 만드세요. -->
<!-- ```{r} -->
<!-- # 감정 범주 변수 생성 -->
<!-- score_comment <- score_comment %>% -->
<!--   mutate(sentiment = ifelse(score >=  1, "pos", -->
<!--                      ifelse(score <= -1, "neg", "neu"))) -->
<!-- score_comment %>% -->
<!--   select(sentiment, reply) -->
<!-- # 감정 범주 빈도 구하기 -->
<!-- frequency_score <- score_comment %>% -->
<!--   count(sentiment) -->
<!-- frequency_score -->
<!-- # 막대 그래프 만들기 -->
<!-- library(ggplot2) -->
<!-- ggplot(frequency_score, aes(x = sentiment, y = n, fill = sentiment)) + -->
<!--   geom_col() + -->
<!--   geom_text(aes(label = n), vjust = -0.3) -->
<!-- ``` -->
<!-- Q4. 댓글을 띄어쓰기 기준으로 토큰화한 다음 감정 범주별 단어 빈도를 구하세요. -->
<!-- ```{r} -->
<!-- # 토큰화 -->
<!-- comment <- score_comment %>% -->
<!--   unnest_tokens(input = reply, -->
<!--                 output = word, -->
<!--                 token = "words", -->
<!--                 drop = F) -->
<!-- # 감정 범주별 단어 빈도 구하기 -->
<!-- frequency_word <- comment %>% -->
<!--   count(sentiment, word, sort = T) -->
<!-- frequency_word -->
<!-- ``` -->
<!-- Q5. 로그 오즈비를 이용해 긍정 댓글과 부정 댓글에 상대적으로 자주 사용된 단어를 10개씩 추출하세요. -->
<!-- ```{r} -->
<!-- # long form을 wide form으로 변환 -->
<!-- library(tidyr) -->
<!-- comment_wide <- frequency_word %>% -->
<!--   filter(sentiment != "neu") %>% -->
<!--   pivot_wider(names_from = sentiment, -->
<!--               values_from = n, -->
<!--               values_fill = list(n = 0)) -->
<!-- comment_wide -->
<!-- # 로그 오즈비 구하기 -->
<!-- comment_wide <- comment_wide %>% -->
<!--   mutate(log_odds_ratio = log(((pos + 1) / (sum(pos + 1))) / -->
<!--                               ((neg + 1) / (sum(neg + 1))))) -->
<!-- comment_wide -->
<!-- # 긍정, 부정 댓글에 상대적으로 자주 사용된 단어 추출 -->
<!-- top10 <- comment_wide %>% -->
<!--   group_by(sentiment = ifelse(log_odds_ratio > 0, "pos", "neg")) %>% -->
<!--   slice_max(abs(log_odds_ratio), n = 10) -->
<!-- top10 -->
<!-- ``` -->
<!-- Q6. 긍정 댓글과 부정 댓글에 상대적으로 자주 사용된 단어를 나타낸 막대 그래프를 만드세요. -->
<!-- ```{r} -->
<!-- ggplot(top10, aes(x = reorder(word, log_odds_ratio), -->
<!--                       y = log_odds_ratio, -->
<!--                       fill = sentiment)) + -->
<!--   geom_col() + -->
<!--   coord_flip() + -->
<!--   labs(x = NULL) -->
<!-- ``` -->
<!-- Q7. 'Q3'에서 만든 데이터를 이용해 '긍정 댓글에 가장 자주 사용된 단어'를 언급한 댓글을 감정 점수가 높은 순으로 출력하세요. -->
<!-- ```{r} -->
<!-- score_comment %>% -->
<!--   filter(str_detect(reply, "자랑스럽다")) %>% -->
<!--   select(reply) %>% -->
<!--   arrange(-score) -->
<!-- ``` -->
<!-- Q8. 'Q3'에서 만든 데이터를 이용해 '부정 댓글에 가장 자주 사용된 단어'를 언급한 댓글을 감정 점수가 낮은 순으로 출력하세요. -->
<!-- ```{r} -->
<!-- score_comment %>% -->
<!--   filter(str_detect(reply, "국내")) %>% -->
<!--   select(reply) %>% -->
<!--   arrange(score) -->
<!-- ``` -->
<!-- # 5장 -->
<!-- `"news_comment_BTS.csv"`에는 2020년 9월 21일 방탄소년단이 '빌보드 핫 100 차트' 1위에 오른 소식을 다룬 기사에 달린 댓글이 들어있습니다. `"news_comment_BTS.csv"`를 이용해 문제를 해결해 보세요. -->
<!-- Q1. `"news_comment_BTS.csv"`를 불러온 다음 행 번호를 나타낸 변수를 추가하고 분석에 적합하게 전처리하세요. -->
<!-- ```{r eval=F} -->
<!-- # 기사 댓글 불러오기 -->
<!-- library(readr) -->
<!-- library(dplyr) -->
<!-- raw_news_comment <- read_csv("news_comment_BTS.csv") -->
<!-- glimpse(raw_news_comment) -->
<!-- ``` -->
<!-- ```{r echo=F} -->
<!-- library(readr) -->
<!-- library(dplyr) -->
<!-- raw_news_comment <- read_csv(here::here("files/news_comment_BTS")) -->
<!-- glimpse(raw_news_comment) -->
<!-- ``` -->
<!-- ```{r} -->
<!-- # 전처리 -->
<!-- library(stringr) -->
<!-- library(textclean) -->
<!-- news_comment <- raw_news_comment %>%  -->
<!--   select(reply) %>%  -->
<!--   mutate(id = row_number(), -->
<!--          reply = str_replace_all(reply, "[^가-힣]", " "),  -->
<!--          reply = str_squish(reply)) -->
<!-- news_comment %>% -->
<!--   select(id, reply) -->
<!-- ``` -->
<!-- Q2. 댓글에서 명사, 동사, 형용사를 추출하고 ‘/로 시작하는 모든 문자’를 ‘다’로 바꾸세요. -->
<!-- ```{r eval=F} -->
<!-- # 품사 기준 토큰화 -->
<!-- library(tidytext)  -->
<!-- library(KoNLP)  -->
<!-- comment_pos <- news_comment %>%  -->
<!--   unnest_tokens(input = reply,  -->
<!--                 output = word,  -->
<!--                 token = SimplePos22,  -->
<!--                 drop = F) -->
<!-- ``` -->
<!-- ```{r echo=F} -->
<!-- # saveRDS(comment_pos, here::here("files_etc/comment_BTS_pos.rds"), compress = F) -->
<!-- library(tidytext)  -->
<!-- library(KoNLP) -->
<!-- comment_pos <- readRDS(here::here("files_etc/comment_BTS_pos.rds")) -->
<!-- ``` -->
<!-- ```{r} -->
<!-- # 한 행이 한 품사를 구성하도록 분리 -->
<!-- library(tidyr) -->
<!-- comment_pos <- comment_pos %>%  -->
<!--   separate_rows(word, sep = "[+]")  -->
<!-- comment_pos %>%   -->
<!--   select(word, reply) -->
<!-- # 명사, 동사, 형용사 추출 -->
<!-- comment <- comment_pos %>% -->
<!--   separate_rows(word, sep = "[+]") %>% -->
<!--   filter(str_detect(word, "/n|/pv|/pa")) %>% -->
<!--   mutate(word = ifelse(str_detect(word, "/pv|/pa"), -->
<!--                        str_replace(word, "/.*$", "다"), -->
<!--                        str_remove(word, "/.*$"))) %>% -->
<!--   filter(str_count(word) >= 2) %>% -->
<!--   arrange(id) -->
<!-- comment %>%  -->
<!--   select(word, reply) -->
<!-- ``` -->
<!-- Q3. 다음 코드를 이용해 유의어를 통일한 다음 한 댓글이 하나의 행이 되도록 단어를 결합하세요. -->
<!-- ```{r echo=T} -->
<!-- # 유의어 통일하기 -->
<!-- comment <- comment %>%   -->
<!--   mutate(word = case_when(str_detect(word, "축하") ~ "축하",  -->
<!--                           str_detect(word, "방탄") ~ "자랑", -->
<!--                           str_detect(word, "대단") ~ "대단", -->
<!--                           str_detect(word, "자랑") ~ "자랑", -->
<!--                           T ~ word)) -->
<!-- ``` -->
<!-- <br>  -->
<!-- ```{r} -->
<!-- # 단어를 댓글별 한 행으로 결합 -->
<!-- line_comment <- comment %>% -->
<!--   group_by(id) %>% -->
<!--   summarise(sentence = paste(word, collapse = " ")) -->
<!-- line_comment -->
<!-- ``` -->
<!-- Q4. 댓글을 바이그램으로 토큰화한 다음 바이그램 단어쌍을 분리하세요.  -->
<!-- ```{r} -->
<!-- # 바이그램 토큰화 -->
<!-- bigram_comment <- line_comment %>% -->
<!--   unnest_tokens(input = sentence, -->
<!--                 output = bigram, -->
<!--                 token = "ngrams", -->
<!--                 n = 2) -->
<!-- bigram_comment -->
<!-- # 바이그램 단어쌍 분리 -->
<!-- bigram_seprated <- bigram_comment %>% -->
<!--   separate(bigram, c("word1", "word2"), sep = " ") -->
<!-- bigram_seprated -->
<!-- ``` -->
<!-- Q5. 단어쌍 빈도를 구한 다음 네트워크 그래프 데이터를 만드세요.  -->
<!-- - 난수를 고정한 다음 네트워크 그래프 데이터를 만드세요. -->
<!-- - 빈도가 3 이상인 단어쌍만 사용하세요. -->
<!-- - 연결 중심성과 커뮤니티를 나타낸 변수를 추가하세요. -->
<!-- ```{r} -->
<!-- # 단어쌍 빈도 구하기 -->
<!-- pair_bigram <- bigram_seprated %>% -->
<!--   count(word1, word2, sort = T) %>% -->
<!--   na.omit() -->
<!-- pair_bigram -->
<!-- # 네트워크 그래프 데이터 만들기 -->
<!-- library(tidygraph) -->
<!-- set.seed(1234) -->
<!-- graph_bigram <- pair_bigram %>% -->
<!--   filter(n >= 3) %>% -->
<!--   as_tbl_graph(directed = F) %>% -->
<!--   mutate(centrality = centrality_degree(), -->
<!--          group = as.factor(group_infomap())) -->
<!-- graph_bigram -->
<!-- ``` -->
<!-- Q6. 바이그램을 이용해 네트워크 그래프를 만드세요. -->
<!-- - 난수를 고정한 다음 네트워크 그래프를 만드세요. -->
<!-- - 레이아웃을 `"fr"`로 설정하세요. -->
<!-- - 연결 중심성에 따라 노드 크기를 정하고, 커뮤니티별로 노드 색깔이 다르게 설정하세요. -->
<!-- - 노드의 범례를 삭제하세요. -->
<!-- - 텍스트가 노드 밖에 표시되게 설정하고, 텍스트의 크기를 5로 설정하세요. -->
<!-- ```{r} -->
<!-- library(ggraph) -->
<!-- set.seed(1234) -->
<!-- ggraph(graph_bigram, layout = "fr") +  -->
<!--   geom_edge_link() + -->
<!--   geom_node_point(aes(size = centrality, -->
<!--                       color = group), -->
<!--                   show.legend = F) + -->
<!--   geom_node_text(aes(label = name), -->
<!--                  repel = T, -->
<!--                  size = 5) + -->
<!--   theme_graph() -->
<!-- ``` -->
<!-- ```{r} -->
<!-- # 그래프 꾸미기 -->
<!-- set.seed(1234) -->
<!-- ggraph(graph_bigram, layout = "fr") +         # 레이아웃 -->
<!--   geom_edge_link(color = "gray50",            # 엣지 색깔 -->
<!--                  alpha = 0.5) +               # 엣지 명암 -->
<!--   geom_node_point(aes(size = centrality,      # 노드 크기 -->
<!--                       color = group),         # 노드 색깔 -->
<!--                   show.legend = F) +          # 범례 삭제 -->
<!--   scale_size(range = c(4, 8)) +               # 노드 크기 범위 -->
<!--   geom_node_text(aes(label = name),           # 텍스트 표시 -->
<!--                  repel = T,                   # 노드밖 표시 -->
<!--                  size = 5) +                  # 텍스트 크기 -->
<!--   theme_graph()                               # 배경 삭제 -->
<!-- ``` -->
<!-- # 6장 -->
<!-- `speeches_roh.csv`에는 노무현 전 대통령의 연설문 780개가 들어있습니다. `speeches_roh.csv`를 이용해 문제를 해결해 보세요. -->
<!-- Q1. `speeches_roh.csv`를 불러온 다음 연설문이 들어있는 `content`를 문장 기준으로 토큰화하세요. -->
<!-- ```{r eval=F} -->
<!-- # 연설문 불러오기 -->
<!-- library(readr) -->
<!-- speeches_raw <- read_csv("speeches_roh.csv") -->
<!-- ``` -->
<!-- ```{r echo=F} -->
<!-- library(readr) -->
<!-- speeches_raw <- read_csv(here::here("files/speeches_roh.csv")) -->
<!-- ``` -->
<!-- ```{r} -->
<!-- # 문장 기준 토큰화 -->
<!-- library(dplyr) -->
<!-- library(tidytext) -->
<!-- speeches <- speeches_raw %>% -->
<!--   unnest_tokens(input = content, -->
<!--                 output = sentence, -->
<!--                 token = "sentences", -->
<!--                 drop = F) -->
<!-- ``` -->
<!-- - [꿀팁] `KoNLP` 패키지의 함수는 토큰화할 텍스트가 너무 길면 오류가 발생합니다. 텍스트를 문장 기준으로 토큰화하고 나서 다시 명사 기준으로 토큰화하면 이런 문제를 피할 수 있습니다. -->
<!-- - [꿀팁] 노무현 전 대통령의 연설문 출처: bit.ly/easytext_35 -->
<!-- Q2. 문장을 분석에 적합하게 전처리한 다음 명사를 추출하세요. -->
<!-- - [꿀팁] 컴퓨터 성능에 따라 명사를 추출하는 데 시간이 오래 걸릴 수 있습니다. -->
<!-- ```{r} -->
<!-- # 전처리 -->
<!-- library(stringr) -->
<!-- speeches <- speeches %>% -->
<!--   mutate(sentence = str_replace_all(sentence, "[^가-힣]", " "), -->
<!--          sentence = str_squish(sentence)) -->
<!-- ``` -->
<!-- ```{r eval=FALSE} -->
<!-- # 명사 추출 -->
<!-- library(tidytext) -->
<!-- library(KoNLP) -->
<!-- library(stringr) -->
<!-- nouns_speeches <- speeches %>% -->
<!--   unnest_tokens(input = sentence, -->
<!--                 output = word, -->
<!--                 token = extractNoun, -->
<!--                 drop = F) %>% -->
<!--   filter(str_count(word) > 1) -->
<!-- ``` -->
<!-- ```{r echo=F} -->
<!-- library(tidytext) -->
<!-- library(KoNLP) -->
<!-- library(stringr) -->
<!-- # saveRDS(nouns_speeches, here::here("files_etc/nouns_speeches_roh.rds"), compress = F) -->
<!-- nouns_speeches <- readRDS(here::here("files_etc/nouns_speeches_roh.rds")) -->
<!-- ``` -->
<!-- Q3. 연설문 내 중복 단어를 제거하고 빈도가 100회 이하인 단어를 추출하세요. -->
<!-- ```{r} -->
<!-- # 연설문 내 중복 단어 제거 -->
<!-- nouns_speeches <- nouns_speeches %>% -->
<!--   group_by(id) %>% -->
<!--   distinct(word, .keep_all = T) %>% -->
<!--   ungroup() -->
<!-- # 단어 빈도 100회 이하 단어 추출 -->
<!-- nouns_speeches <- nouns_speeches %>% -->
<!--   add_count(word) %>% -->
<!--   filter(n <= 100) %>% -->
<!--   select(-n) -->
<!-- ``` -->
<!-- Q4. 추출한 단어에서 다음의 불용어를 제거하세요. -->
<!-- ```{r include=F} -->
<!-- knitr::opts_chunk$set(echo = T, eval = T, warning = F, message = F) -->
<!-- library(here) -->
<!-- ``` -->
<!-- ```{r eval=T, echo=T} -->
<!-- stopword <- c("들이", "하다", "하게", "하면", "해서", "이번", "하네", -->
<!--               "해요", "이것", "니들", "하기", "하지", "한거", "해주", -->
<!--               "그것", "어디", "여기", "까지", "이거", "하신", "만큼") -->
<!-- ``` -->
<!-- <br> -->
<!-- ```{r} -->
<!-- nouns_speeches <- nouns_speeches %>% -->
<!--   filter(!word %in% stopword) -->
<!-- ``` -->
<!-- Q5. 연설문별 단어 빈도를 구한 다음 DTM을 만드세요. -->
<!-- ```{r } -->
<!-- # 연설문별 단어 빈도 구하기 -->
<!-- count_word_doc <- nouns_speeches %>% -->
<!--   count(id, word, sort = T) -->
<!-- # DTM 만들기 -->
<!-- dtm_comment <- count_word_doc %>% -->
<!--   cast_dtm(document = id, term = word, value = n) -->
<!-- ``` -->
<!-- Q6. 토픽 수를 2~20개로 바꿔가며 LDA 모델을 만든 다음 최적 토픽 수를 구하세요. -->
<!-- ```{r eval=F} -->
<!-- # 토픽 수 바꿔가며 LDA 모델 만들기 -->
<!-- library(ldatuning) -->
<!-- models <- FindTopicsNumber(dtm = dtm_comment, -->
<!--                            topics = 2:20, -->
<!--                            return_models = T, -->
<!--                            control = list(seed = 1234)) -->
<!-- ``` -->
<!-- ```{r echo=F} -->
<!-- library(ldatuning) -->
<!-- # saveRDS(models, here::here("files_etc/models_roh.rds"), compress = F) -->
<!-- models <- readRDS(here::here("files_etc/models_roh.rds")) -->
<!-- ``` -->
<!-- ```{r} -->
<!-- # 최적 토픽 수 구하기 -->
<!-- FindTopicsNumber_plot(models) -->
<!-- ``` -->
<!-- Q7. 토픽 수가 9개인 LDA 모델을 추출하세요. -->
<!-- ```{r} -->
<!-- lda_model <- models %>% -->
<!--   filter (topics == 9) %>% -->
<!--   pull(LDA_model) %>% -->
<!--  .[[1]] -->
<!-- ``` -->
<!-- Q8. LDA 모델의 beta를 이용해 각 토픽에 등장할 확률이 높은 상위 10개 단어를 추출한 다음 토픽별 주요 단어를 나타낸 막대 그래프를 만드세요. -->
<!-- ```{r} -->
<!-- # beta 추출 -->
<!-- term_topic <- tidy(lda_model, matrix = "beta") -->
<!-- # 토픽별 beta 상위 단어 추출 -->
<!-- top_term_topic <- term_topic %>% -->
<!--   group_by(topic) %>% -->
<!--   slice_max(beta, n = 10) -->
<!-- # 막대 그래프 만들기 -->
<!-- library(ggplot2) -->
<!-- ggplot(top_term_topic, -->
<!--        aes(x = reorder_within(term, beta, topic), -->
<!--            y = beta, -->
<!--            fill = factor(topic))) + -->
<!--   geom_col(show.legend = F) + -->
<!--   facet_wrap(~ topic, scales = "free", ncol = 3) + -->
<!--   coord_flip () + -->
<!--   scale_x_reordered() + -->
<!--   labs(x = NULL) -->
<!-- ``` -->
<!-- Q9. LDA 모델의 gamma를 이용해 연설문 원문을 확률이 가장 높은 토픽으로 분류하세요. -->
<!-- ```{r} -->
<!-- # gamma 추출 -->
<!-- doc_topic <- tidy(lda_model, matrix = "gamma") -->
<!-- # 문서별로 확률이 가장 높은 토픽 추출 -->
<!-- doc_class <- doc_topic %>% -->
<!--   group_by(document) %>% -->
<!--   slice_max(gamma, n = 1) -->
<!-- # 변수 타입 통일 -->
<!-- doc_class$document <- as.integer(doc_class$document) -->
<!-- # 연설문 원문에 확률이 가장 높은 토픽 번호 부여 -->
<!-- speeches_topic <- speeches_raw %>% -->
<!--   left_join(doc_class, by = c("id" = "document")) -->
<!-- ``` -->
<!-- Q10. 토픽별 문서 수를 출력하세요. -->
<!-- ```{r} -->
<!-- speeches_topic %>% -->
<!--   count(topic) -->
<!-- ``` -->
<!-- Q11. 문서가 가장 많은 토픽의 연설문을 gamma가 높은 순으로 출력하고 내용이 비슷한지 살펴보세요. -->
<!-- ```{r} -->
<!-- speeches_topic %>% -->
<!--   filter(topic == 9) %>% -->
<!--   arrange(-gamma) %>% -->
<!--   select(content) -->
<!-- ``` -->
