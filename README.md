<h1>A/B-тест: новая система рекомендации постов</h1>

<p>Разработан новый алгоритм рекоменадции постов в ленте.</p>

<p>Принято решение провести A/B-тест, чтобы проверить увеличился ли CTR в группе с новым алгоритмом.</p>

<p>Экспермент проводился на пользователях в группах 1 и 2:</p>
<ul>
  <li>группа 1 использовалась в качестве контроля;</li>
  <li>на группе 2 использован один из новых алгоритмов рекомендации постов.</li>
</ul>

<p>В данном случае, пользователи уже разделены на группы, сроки эксперимента определены, поэтому необходимо только проверить есть ли статистически значимые различие между группами.</p>

<h2>Структура данных в ClickHouse</h2>
<h3>Таблица feed_actions</h3>
<table>
  <thead>
    <tr>
      <th>user_id</th>
      <th>post_id</th>
      <th>action</th>
      <th>time</th>
      <th>gender</th>
      <th>age</th>
      <th>country</th>
      <th>city</th>
      <th>os</th>
      <th>source</th>
      <th>exp_group</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>139605</td>
      <td>2243</td>
      <td>view</td>
      <td>07/03/24 00:00</td>
      <td>1</td>
      <td>23</td>
      <td>Russia</td>
      <td>Birsk</td>
      <td>iOS</td>
      <td>organic</td>
      <td>0</td>
    </tr>
    <tr>
      <td>138505</td>
      <td>2436</td>
      <td>view</td>
      <td>07/03/24 00:00</td>
      <td>0</td>
      <td>24</td>
      <td>Russia</td>
      <td>Rostov</td>
      <td>Android</td>
      <td>organic</td>
      <td>1</td>
    </tr>
    <tr>
      <td>140314</td>
      <td>2436</td>
      <td>like</td>
      <td>07/03/24 00:00</td>
      <td>1</td>
      <td>19</td>
      <td>Russia</td>
      <td>Yelizovo</td>
      <td>Android</td>
      <td>organic</td>
      <td>3</td>
    </tr>
  </tbody>
</table>

<h3>Таблица message_actions</h3>
<table>
  <thead>
    <tr>
      <th>user_id</th>
      <th>receiver_id</th>
      <th>time</th>
      <th>source</th>
      <th>exp_group</th>
      <th>gender</th>
      <th>age</th>
      <th>country</th>
      <th>city</th>
      <th>os</th>     
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>112866</td>
      <td>110282</td>
      <td>07/03/24 00:00</td>
      <td>organic</td>
      <td>3</td>
      <td>1</td>
      <td>29</td>
      <td>Russia</td>
      <td>Moscow</td>
      <td>Android</td>
    </tr>
    <tr>
      <td>111607</td>
      <td>113653</td>
      <td>07/03/24 00:00</td>
      <td>organic</td>
      <td>1</td>
      <td>1</td>
      <td>20</td>
      <td>Russia</td>
      <td>Barnaul</td>
      <td>Android</td>
    </tr>
    <tr>
      <td>7401</td>
      <td>7800</td>
      <td>07/03/24 00:00</td>
      <td>ads</td>
      <td>0</td>
      <td>0</td>
      <td>17</td>
      <td>Azerbaijan</td>
      <td>Ağdaş</td>
      <td>iOS</td>
    </tr>
  </tbody>
</table>


<h2>Методы анализа</h2>

<p>Перед проведением анализа проверим систему сплитования, проведя А/А тест.</p>

<p>В ознакомительных целях приверим влияние новго алгоритма на CTR различными способами:</p>

<ul>
  <li>t-тест</li>
  <li>Пуассоновский бутстреп</li>
  <li>тест Манна-Уитни</li>
  <li>t-тест на сглаженном ctr (α=5)</li>
  <li>t-тест и тест Манна-Уитни поверх бакетного преобразования</li>
</ul>

