유튜브 데이터로 무엇을 할 것인가?
우리 팀은 유튜브 데이터를 활용하여 부업으로 유튜브를 시작하는 사람에게 일종의 가이드라인을 제시하고자 했습니다.

특히, 어떤 카테고리가 가장 경쟁이 치열한지, 어떤 카테고리로 시작하는게 좋을지 추천을 해보기로 계획했습니다.

폰트 설정 및 패키지 불러오기
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.font_manager as fm
import seaborn as sns

font_location = 'C:\\Users\\CPB06GameN\\Downloads\\NanumFontSetup_TTF_GOTHIC\\NanumGothic.ttf'
fm._rebuild()
fm.get_fontconfig_fonts()
font_name = fm.FontProperties(fname=font_location).get_name()
plt.rcParams['axes.unicode_minus'] = False 
plt.rcParams['font.family'] = font_name

%matplotlib inline
plt.style.use('gadfly')
데이터 불러오기
df = pd.read_csv('C:\\ff\\KR_youtube_trending_data.csv')

df_new = df[['dislikes','channelTitle', 'categoryId']].copy()

df_new.info()
df_new.describe()
데이터셋
데이터셋2

2020년 8월 초부터 2021년 5월 중순까지 올라온 youtube 영상을 분석하기 위해

3가지 가설을 세웠습니다.

첫째, People & Blog 카테고리 영상이 가장 경쟁이 치열할 것이다. 따라서 경쟁이 아주 치열한 레드오션이므로 부업으로 시작하려는 사람에게는 권하지 않는다.

둘째, Music 카테고리 영상이 가장 Dislike를 많이 받았을 것이다. 그러므로 부업 유튜버가 이 카테고리 영상을 올릴 경우, 스트레스를 받을 가능성이 높을 것이다. 따라서 이 카테고리는 추천하지 않는다.

셋째, Entertainment 카테고리 영상이 가장 많은 Comment를 받을 것이다. 그러므로 이 카테고리는 서로 피드백이 활발할 가능성이 높으므로, 부업으로 유튜브를 하는 사람도 중간에 그만두지 않고 계속 할 수 있을 것이다

그리고 확인하기 위해 차례대로 시각화를 시도해 가설을 확인해 보겠습니다.

가장 많은 영상이 올라온 카테고리는 어디인지 분석하겠습니다.
(평균적으로) 가장 싫어요를 많이 받은 카테고리는 무슨 카테고리 인지 시각화해보겠습니다.
(평균적으로) 가장 댓글을 많이 받은 카테고리는 무슨 카테고리인지 탐색해보겠습니다.
youtube 영상 중 가장 많은 카테고리 차지하는 영상은?
cate_freq = df_new.groupby('categoryId').count()
sort_cate_freq = cate_freq.sort_values(by='dislikes', ascending=False)
sort_cate_freq['idx'] = sort_cate_freq.index
sort_cate_freq = sort_cate_freq.reset_index(drop=True)
sort_cate_freq['idx'] = sort_cate_freq['idx'].astype('str')
lst = sort_cate_freq['idx']

lst_yt = ['Entertaiment', 'People & Blogs', 'Music', 'Sports', 'News & Politics', 'Comedy', 'Film & Animation', 'Howto & Style',
 'Gaming', 'Education', 'Pets & Animals','Science & Technology', 'Autos & Vehicles', 'Travel & Events', 'Nonprofits & Activism']

plt.bar(sort_cate_freq.index, sort_cate_freq['dislikes'], color=gg_palette)
plt.xticks(sort_cate_freq.index, lst_yt, rotation=19, fontsize=9)
plt.title('유튜브 카테고리별 채널 수')
유튜브 카테고리

dislike 가장 많이 받은 카테고리는??
pivot = pd.pivot_table(df_new, values=['dislikes'], index=['categoryId'])
dis = pivot.sort_values(by='dislikes', ascending=True)
dis['idx'] = dis.index
dis = dis.reset_index(drop=True)

lst2 = dis['idx']
lst2_yt = ['Music', 'Science & Technology','Gaming','Comedy','People & Blogs', 'Entertaiment','Autos & Vehicles','Sports','News & Politics','Nonprofits & Activism','Film & Animation','Education','Travel & Events','Howto & Style',
'Pets & Animals']
lst2_yt = lst2_yt[::-1]

plt.barh(dis.index, dis['dislikes'], color=gg_palette)
plt.yticks(dis.index, lst2_yt, fontsize=9)
plt.title('Dislike를 많이 받은 카테고리는?')
Dislike를 많이 받은 카테고리는

가장 댓글을 많이 받은 카테고리는??
df=pd.read_csv('/content/drive/MyDrive/csv/KR_youtube_trending_data.csv', engine='python', error_bad_lines=False)
df1=df[['categoryId','comment_count']]
df_comment=df2.groupby('categoryId').mean().sort_values(by='comment_count', ascending=True)
df_comment.plot(kind='bar')

plt.barh(df_comment.index, df_comment['comment_count'])
plt.title('카테고리ID에 따른 댓글수')
plt.ylabel('카테고리ID')
plt.xlabel('댓글수')
plt.tight_layout()
plt.show()
image
