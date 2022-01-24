# Chatbot
pip install nltk
pip install newspaper3k

#importingf libraries

from newspaper import Article 
import random
import string
import nltk
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.metrics.pairwise import cosine_similarity 
import numpy as np
import warnings
warnings.filterwarnings('ignore')

#download the punkt package
nltk.download('punkt', quiet=True)

print(corpus)

#tokenization
text = corpus
sentence_list = nltk.sent_tokenize(text)

#print the list of sentence
print(sentence_list)

#A function to return a random greeting response to a user greeting
def greeting_response(text):
  text= text.lower()
  #bots greeting response 
  bot_greetings = ['howdy','hy', 'hi', 'hello', 'hola']
  #user greetings
  user_greetings = ['howdy','hy', 'hi', 'hello', 'hola','wassup']

  for word in text.split():
    if word in user_greetings:
      return random.choice(bot_greetings)

def index_sort(list_var):
  length = len(list_var)
  list_index = list(range(0,length))
  
  x= list_var
  for i in range (length):
    for j in range (length):
      if x[list_index[i]] > x[list_index[j]]:
        #swap
        temp = list_index[i]
        list_index[i] = list_index[j]
        list_index[j] = temp
  return list_index

#create bots response
def bot_response(user_input):
  user_input = user_input.lower()
  sentence_list.append(user_input)
  bot_response= ''
  cm = CountVectorizer().fit_transform(sentence_list)

  similarity_score = cosine_similarity(cm[-1],cm)
  similarity_scores_list =  similarity_score.flatten()
  index = index_sort(similarity_scores_list)
  index = index[1:]
  response_flag = 0
  
  j= 0
  for i in range (len(index)):
    if similarity_scores_list[index[i]]>0.0:
      bot_response = bot_response+ '' + sentence_list[index[i]]
      response_flag = 1
      j= j+1 
    if j>2 :
      break
    
  if response_flag == 0:
    bot_response = bot_response + '' + "I apologize, I don't understand"

    sentence_list.remove(user_input)
  return bot_response

#Start the Chat 
print('AIZY : I am AIZY and I am here to help you with Artificial Intelligence')
exit_list = ['exit', 'see you later','bye','quit']
while(True):
  user_input= input()
  if user_input.lower() in exit_list:
    print('AIZY: Chat with you Later!')
    break 
  else: 
    if greeting_response(user_input) != None:
      print('AIZY:'+ greeting_response(user_input))
    else:
      print('AIZY: '+ bot_response(user_input))
