# ETtoday Interview Questions (the department of Big Data and Analytics)
my Answers for Interview Questions in 2020.03 (two SQL questions and one DataViz question)

-- 1st question  
請設計 SQL，使用左邊的兩個table 計算出每個 tag 與 score 的不重複 cookie 人數，  
需顯示無資料的人數(註：tag_score有3種tag ，tag_index_mapping有5種tag)   
並用 index, tag, score 做升冪排序，產出結果如 sheet: Expected Outcome 之 A：D 欄  
  
SELECT ti.idx AS "index", ti.tag AS "tag", ts.score AS "score", COUNT(DISTINCT cookie_id) AS "cookie_times"  
FROM tag_index_mapping ti  
LEFT JOIN tag_score ts  
ON ti.tag = ts.tag  
GROUP BY ti.idx, ti.tag , ts.score  
ORDER BY ti.idx ASC, ti.tag ASC, CAST(ts.score AS INTEGER) ASC;  
  
![image](https://github.com/AREKKUSU-hyper/Interview-Questions-for-ETtoday/blob/master/data%20analytics%20SQL%20test/Q1.png)  
  
-- 2nd question  
請設計 SQL 計算各 tag, score 的人數佔比，產出結果如 sheet: Expected Outcome 之 E 欄  
ex: 頻道-房產 總人數為 212 人，則 E4 的值為 149 / 212，佔比 70.28%  
  
WITH sub AS  
(SELECT ti.tag AS "tag", COUNT(DISTINCT(ts.cookie_id)) AS "sub_cookie"  
FROM tag_index_mapping ti  
LEFT JOIN tag_score ts  
ON ti.tag = ts.tag  
GROUP BY ti.tag  
) 
  
SELECT ti.idx AS "index", ti.tag AS "tag", ts.score AS "score",  
COUNT(DISTINCT ts.cookie_id) AS "cookie_times",  
(printf("%.2f",  COUNT(DISTINCT ts.cookie_id)* 100.00/sub.sub_cookie) || '%') AS "cookie_percentage"  
  
FROM tag_index_mapping ti  
LEFT JOIN tag_score ts  
ON ti.tag = ts.tag  
LEFT JOIN sub  
ON sub.tag = ti.tag  
GROUP BY ti.idx, ti.tag , ts.score, sub.tag, sub.sub_cookie  
ORDER BY ti.idx ASC, ti.tag ASC, CAST(ts.score AS INTEGER) ASC;  
  
![image](https://github.com/AREKKUSU-hyper/Interview-Questions-for-ETtoday/blob/master/data%20analytics%20SQL%20test/Q2.png)  

  
![image](https://github.com/AREKKUSU-hyper/Interview-Questions-for-ETtoday/blob/master/data%20analytics%20DataViz%20test/Q3.png)
