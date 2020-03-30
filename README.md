# COMM7390-Twitter-Data-Mining-on-5G
COMM7390 Group Project-Group a
## Group member
- YANG AILIN
- CHU BINGYANG
- JIANG JIEYI
- ZHONG ZHILING
## Introduction
As the latest generation of cellular network technology, 5G has become one of the hottest topics on the international stage.
With the development of the mobile Internet, more and more devices are connected to the mobile network, and new services and applications have appeared. Therefore, the demand for 5G is growing and is reflected in the fields of high-definition live broadcast, transportation and virtual reality, which has now entered the application stage. We can say that the 5G curtain has opened.
At this stage, 5G technology has been applied in many countries. For example, in mainland China, Huawei has started producing products that support 5G technology. In addition, Huawei also vigorously promotes 5G technology in Europe and is willing to build a 5G technology platform for partners. But we also heard some objections, such as questioning the safety and reliability of 5G applications.
But, as shown by the media news, do most people have a negative attitude towards 5G packages? Therefore, based on a Twitter dataset we crawled, we analyzed the public's attitudes and views on 5G technology.
## Procedures

- Step 0: Creating a Twitter developer account
To start with, we applied for a Twitter developer account and obtain certificates to access the Twitter API. 

- Step 1: Preparation for Visualization 
Then, we installed and load ‘rtweet’ package on R.

We set a command search_tweets used to search Twitter for posts containing hashtag ‘#5G’and set ‘n = 10000’ to access 10000 pieces of tweets. 

We set a command data.frame to make sure objects returned are in the format of data frame and made a colnames command to show all the columns of the data frame.For the project, the attributes we would collect in the following work  are [‘hashtags’, ‘’text’ ]


- Step 2: Visualizing frequency of tweets over time 
We used tools like ‘ggplot2’ packages for visualizing frequency of tweets over time.To show the change on  Twitter statuses from past 9 days, We plot time series of tweets with a line chart by command ‘ts_plot’.And we also used three-hour intervals for more detailed display of time series of tweets.


- Step 3: Text mining and word cloud 
To fulfill the text mining and word cloud, we install several packages, including package’tm’ for text mining, package ‘SnowballC’ for text stemming and package ‘wordcloud’  for word-cloud generating.

After installing these packages on R, we had text mining on the tweets we collected:
Firstly, we loaded the tweets data as a corpus using VectorSource.
Secondly,we used tm_map command to convert the text to lower case and remove numbers, common stopwords,punctuations and white spaces.
Thirdly, we created a matrix using the data we reprocessed in the last step.Then, we set the seed of R's random number generator with a set.seed command.
Finally, we plot the wordcloud with the ‘wordcloud’ package to display the frequency of each word shown in the twitter data.

Similar as above, we had data mining on the hashtags shown with  ‘5G’. We listed the former 20 hashtags using head command.


- Step 4: calculating sentiments of tweets
We used tools like 'RSentiment' packages, 'DT' packages and 'syuzhet' packages for preparation of calculating  sentiments of twitter text.
Before the calculation of sentiment on twitter text, the data of tweets should be preprocessed using tm_map to convert the text to lower case and remove numbers, punctuations and white spaces, then should be converted to matrix with a DocumentTermMatrix function.

The following procedure is to classify the tweets into two  sentiments: 'Positive' and 'Negative’, and to draw the wordcloud of Positive tweets and Negative tweets respectively.

Another method we calculate sentiments is to use ‘syuzhet’  package.We  can  utilize the ‘get_nrc_sentiment’ function to define the sentiment of each tweets .NRC sentiment dictionary contains 10 kinds of sentiments,namely ‘anger’, ‘anticipation’,’disgust’,’fear’,’joy’,’sadness’,’surprise’,’trust’,’negative’,’positive’. And we can plot a bar chart using barplot function to show the scores of each sentiment for the tweets corpus.

## Attached is the R codes:

- Step 0: Creating a Twitter developer account

- Step 1: Preparation for Visualization 
#load rtweet package

> if (!require(‘rtweet’)) install.packages(‘rtweet’) 

> library(‘rtweet’) 

> dataset <- search_tweets(‘#5G’, n = 10000, 
               include_rts = FALSE, lang = ‘en’) 

> dataset 

> data.frame(dataset)

#Show all the columns of the data frame 

> colnames(dataset) 
 [1] ‘user_id’                
 [2] ‘status_id’              
 [3] ‘created_at’             
 [4] ‘screen_name’            
 [5] ‘text’                   
 [6] ‘source’                 
 [17] ‘hashtags’               
 ......
[88] ‘profile_banner_url’     
[89] ‘profile_background_url’ 
[90] ‘profile_image_url’

- Step 2: Visualizing frequency of tweets over time 
#load ggplot2 package

> install.packages(‘ggplot2’)

> library(‘ggplot2’) 

#Plot time series of tweets using ts_plot
> ts_plot(dataset, ‘3 hours’) 

+ ggplot2::theme_minimal() 

+ ggplot2::theme(plot.title = ggplot2::element_text(face = ‘bold’)) 

+ ggplot2::labs( x = NULL, y = NULL, 
title = ‘Frequency of #5G Twitter statuses from past 9 days’, 
subtitle = ‘Twitter status (tweet) counts aggregated using three-hour intervals’, 
caption = ‘\nSource: Data collected from Twitter's REST API via rtweet’ )

 
- Step 3: Text mining and word cloud 

#load tm/ SnowballC/ wordcloud/ RColorBrewer packages

> if (!require(‘tm’))install.packages(‘tm’) 

> if (!require(‘SnowballC’)) install.packages(‘SnowballC’) 

> if (!require(‘wordcloud’))install.packages(‘wordcloud’) 

> if (!require(‘RColorBrewer’))install.packages(‘RColorBrewer’) 

> library(‘tm’)

> library(‘SnowballC’) 

> library(‘wordcloud’) 

> library(‘RColorBrewer’) 

#Load the tweets data as a corpus

> mt.v <- VectorSource(dataset$text) 

> mt.c <- SimpleCorpus(mt.v) 

#Inspect the content of the document 'mt.c'

> inspect(mt.c) 

> mt.c.p <- tm_map(mt.c, content_transformer(tolower))

#Preprocess the corpus

> mt.c.p <- tm_map(mt.c.p, removeNumbers) 

> mt.c.p <- tm_map(mt.c.p, removeWords, stopwords(‘english’)) 

> mt.c.p <- tm_map(mt.c.p, removePunctuation)

> mt.c.p <- tm_map(mt.c.p, stripWhitespace) 

#Inspect the content of the document 'mt.c.p'

> inspect(mt.c.p) 

#Constructs a matrix

> dtm <- TermDocumentMatrix(mt.c.p)

> m <- as.matrix(dtm)

> v <- sort(rowSums(m),decreasing=TRUE)

> d <- data.frame(word = names(v),freq=v)

#Show the former 20 items in the dataset

> head(d, 20)

                                         word freq
iot                                       iot 1650
will                                     will 1641
technology                         technology 1028
new                                       new 1011
amp                                       amp  970
network                               network  869
via                                       via  792
mobile                                 mobile  696
innovation                         innovation  649
cloud                                   cloud  595
can                                       can  580
security                             security  578
huawei                                 huawei  566
networks                             networks  547
wireless                             wireless  517
tech                                     tech  506
future                                 future  461
industry                             industry  445
first                                   first  402
business                             business  400 

#Set the seed of R's random number generator 

> set.seed(1234) 

#Plot the wordcloud of the tweets 

> wordcloud(words = d$word, 
freq = d$freq, 
min.freq = 1, 
max.words=200, 
random.order=FALSE, 
rot.per=0.0, 
colors=brewer.pal(8, ‘Dark2’))
 

#Load the hashtags data as a corpus

> ha.v1 <- VectorSource(dataset$hashtags) 

> ha.c1 <- SimpleCorpus(ha.v1) 

#Inspect the content of the document 'ha.c1'

> inspect(ha.c1) 

#Preprocess the hashtags data

> ha.c.p1 <- tm_map(ha.c1, content_transformer(tolower)) 

> ha.c.p1 <- tm_map(ha.c.p1, removeNumbers) 

> ha.c.p1 <- tm_map(ha.c.p1, removePunctuation)

> ha.c.p1 <- tm_map(ha.c.p1, stripWhitespace) 

#Inspect the content of the document 'ha.c.p1' 

> inspect(ha.c.p1) 

#Constructs a matrix

> dtm1 <- TermDocumentMatrix(ha.c.p1)

> m1 <- as.matrix(dtm1)

> v1 <- sort(rowSums(m1),decreasing=TRUE)

> d1 <- data.frame(word = names(v1),freq=v1)

#Show the 20 hashtags connected to '#5G'

> head(d1, 20)
                                         word freq
iot                                       iot 1261
innovation                         innovation  420
digitaltransformation   digitaltransformation  400
cloud                                   cloud  398
technology                         technology  382
bigdata                               bigdata  354
teamericsson                     teamericsson  334
tech                                     tech  300
cybersecurity                   cybersecurity  276
mobile                                 mobile  262
security                             security  257
machinelearning               machinelearning  254
smartcities                       smartcities  249
ciot                                     ciot  230
artificialintelligence artificialintelligence  226
robotics                             robotics  222
blockchain                         blockchain  213
smartcity                           smartcity  197
huawei                                 huawei  189
network                               network  171

Step 4: calculating sentiments of tweets

> install.packages('RSentiment')

> install.packages('DT')

> install.packages('syuzhet')

#Load the tweets as a corpus

> r1 = as.character(dataset$text)

#Preprocess the hashtags data
> set.seed(100)

> sample = sample(r1, (length(r1)))

> corpus = Corpus(VectorSource(list(sample)))

> corpus = tm_map(corpus, removePunctuation)

> corpus = tm_map(corpus, content_transformer(tolower))

> corpus = tm_map(corpus, removeNumbers)

> corpus = tm_map(corpus, stripWhitespace)

> corpus = tm_map(corpus, 
removeWords, stopwords('english'))

> corpus = tm_map(corpus, stemDocument)

> dtm_up = DocumentTermMatrix(VCorpus(
VectorSource(corpus[[1]]$content)))

> freq_up <- colSums(as.matrix(dtm_up))

#Calculate sentiments of tweets

> library(‘RSentiment’)

> sentiments_up = calculate_sentiment(names(freq_up))

> sentiments_up = cbind(sentiments_up, 
 as.data.frame(freq_up))
 
> sent_pos_up = sentiments_up[
                   sentiments_up$sentiment == 'Positive',]
                   
> sent_neg_up = sentiments_up[
                   sentiments_up$sentiment == 'Negative',]
                   
> cat(‘We have far lower negative Sentiments: ‘,
sum(sent_neg_up$freq_up),’ than positive: ‘,
sum(sent_pos_up$freq_up))

#Creat a datatable for 'positive'tweets

> library(‘DT’)

> DT::datatable(sent_pos_up)
 
#Plot the wordcloud of the 'positive'tweets 

>layout(matrix(c(1, 2), nrow=2), heights=c(1, 4))

>par(mar=rep(0, 4))

>plot.new()

>set.seed(100)

>wordcloud(sent_pos_up$text,sent_pos_up$freq,min.freq=10,colors=brewer.pal(6,’Dark2’))
 

#Plot the datatable of the 'negative' tweets 

> DT::datatable(sent_neg_up)
 
> plot.new()

> set.seed(100)

> wordcloud(sent_neg_up$text,sent_neg_up$freq, 
min.freq=10,colors=brewer.pal(6, ‘Dark2’))

 
#Load the syuzhet packsge

> text = as.character(rangoon$text) 

#Remove reply

> some_txt<-gsub(‘(RT|via)((?:\\b\\w*@\\w+)+)’,’’,text)

#Remove links

> some_txt<-gsub(‘http[^[:blank:]]+’,’’,some_txt)

#Remove names

> some_txt<-gsub(‘@\\w+’,’’,some_txt)

#Remove punctuations

> some_txt<-gsub(‘[[:punct:]]’,’ ‘,some_txt)

#Remove numbers

> some_txt<-gsub(‘[^[:alnum:]]’,’ ‘,some_txt)

#Load the get_nrc_sentiment function

> library(‘ggplot2’)

> library(‘syuzhet’)

> mysentiment<-get_nrc_sentiment((some_txt))

#Calculate sentiments

> mysentiment.positive =sum(mysentiment$positive)

> mysentiment.anger =sum(mysentiment$anger)

> mysentiment.anticipation =sum(mysentiment$anticipation)

> mysentiment.disgust =sum(mysentiment$disgust)

> mysentiment.fear =sum(mysentiment$fear)

> mysentiment.joy =sum(mysentiment$joy)

> mysentiment.sadness =sum(mysentiment$sadness)

> mysentiment.surprise =sum(mysentiment$surprise)

> mysentiment.trust =sum(mysentiment$trust)

> mysentiment.negative =sum(mysentiment$negative)

#Plot the barchart of the scores of sentiments using barplot function 

> yAxis <- c(mysentiment.positive,

+ mysentiment.anger,
+ mysentiment.anticipation,
+ mysentiment.disgust,
+ mysentiment.fear,
+ mysentiment.joy,
+ mysentiment.sadness,
+ mysentiment.surprise,
+ mysentiment.trust,
+ mysentiment.negative)

> xAxis <- c(‘Positive’,’Anger’,’Anticipation’,
 ‘Disgust’,’Fear’,’Joy’,’Sadness’,
 ‘Surprise’,’Trust’,’Negative’)
 
> colors <- c(‘green’,’red’,’blue’,
‘orange’,’red’,’green’,’orange’,
‘blue’,’green’,’red’)

> yRange <- range(0,yAxis) + 1000

> barplot(yAxis, names.arg = xAxis,
 xlab = ‘Emotional valence’,ylab = ‘Score’,
 main = ‘Twitter sentiment for 5G’,
 sub = ‘Nov 2019’, col = colors,border = ‘black’,       
 ylim = yRange, xpd = F, axisnames = T, 
 cex.axis = 0.8, cex.sub = 0.8, col.sub = ‘blue’)
 
> colSums(mysentiment)
       anger anticipation      disgust         fear 
        2121         6901          928         2791 
         joy      sadness     surprise        trust 
        2394         1387         2195         6033 
    negative     positive 
        3802        12744 


