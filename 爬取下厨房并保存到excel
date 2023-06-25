import requests
import openpyxl
# ,csv
# 使用beautifusoup来解析数据，使用csv或者openpyxl来存储数据
from bs4 import BeautifulSoup
from notion_client import Client

notion = Client(auth="secret_8eQrKQM7M3XZZ4ziIi0WwUONZCZdwRuSJihojEEXLQ5")
database_id = "ec47a378ac124ebfaa941e52a4b27169"

# 获取解析数据
# 获取所有目标url，本周最受欢迎菜谱里一共有20页，观察网址我们发现，只需要通过改变链接末尾的page=的数值即可实现翻页。
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.96 Safari/537.36'}
# 添加request headers,伪装成浏览器登录，若不添加则会被浏览器认出来是爬虫，而有的浏览器会限制爬虫，比如下厨房。
foods_list = []  # 存储食物数据
for i in range(1, 21):
    url = 'https://www.xiachufang.com/explore/?page=' + str(i)  # 通过改变i的数值达到爬取所有网页的目的
    res = requests.get(url, headers=headers)  # 获取数据
    soup = BeautifulSoup(res.text, 'html.parser')  # 解析数据
    inf = soup.find_all('div', class_="recipe recipe-215-horizontal pure-g image-link display-block")  # 找到最小父级共同标签
for food in inf:
    food_name = food.find('img')['alt']  # 菜名
    food_ingredients = food.find('p', class_='ing ellipsis').text  # 食材
    food_href = 'https://www.xiachufang.com' + food.find('a')['href']  # 链接
    food_author = food.find(class_='author').text  # 作者
    foods_list.append([food_name, food_href, food_ingredients])  # 把获取的数据添加到列表
    print('菜名:\t%s\n'
          '用料:\t%s'
          '链接:\t%s\n'
          '作者:\t%s\n'
          '\n' % (food_name, food_ingredients, food_href, food_author))  # 打印
#存储到excel
wb = openpyxl.Workbook() #创建工作薄
sheet = wb.active #获取工作薄活动表
sheet.title = 'menu' #命名
headers = ['菜品','URL','用料'] #表头
sheet.append(headers)
for food in foods_list:
    sheet.append(food) #添加数据
wb.save('xiachufang1.xlsx') #保存
