# 歸入權計算器

<a target="_blank" href="https://colab.research.google.com/github/bwjoik/disgorgement_calculator">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

---

這是一個贖罪的project，我在111學年度下學期東吳大學法律學系莊永丞教授法四D開設的證券交易法的期末考上，算錯歸入權。

這個project聚焦在中華民國證券交易法規定之下，法律相關考題的歸入權計算問題。

想要變更資料的話，請參考data_template.csv，請且在code中"資料導入"處更改導入檔案名，資料必須與主程式位於同個路徑之下，或者直接指定路徑。

---

這個project有兩個主要功能：

1. 交易期間處理：可以判斷交易是否超過證交法§157條之六個月期間(以180日計算，未必合於民法期日之規定)
2. 價格與股數處理：可以計算，並產出歸入權總和

這個project目前不能做到的：

1. 有價證券種類不同者的轉換：依照證券交易法施行細則第11條第2項，需要先行手動轉換
2. 出現重複價格的複數交易：不確定是否能夠處理，考題較不常出現，待改善

---
1. date_of_trade:交易時間
2. type_of_trade:交易類型(買入或賣出)
3. purchase_share:買入股數
4. purchase_price:買入股價
5. sale_share:賣出股數
6. sale_price:買出股價
---
### 計算邏輯

1. 將資料分成purchase_data、sale_data
2. 從purchase_data取出最高賣出價格那筆交易(higheset_sale)
3. 將highest_sale與所有的sale_data的date_of_trade比較，將沒有超過180天的sale_data加入target_purchase
4. 刪除重複 drop duplicate in target_purchase(not sure why exist)
5. 從target_purchase中取出最低股價者 get the data with the lowest purchase_price in target_purchase
6. 比較highest_sale跟lowest_purchase的股數(sale_share、purchase_share)
    1. 如果股數highest_sale < lowest_purchase 且 股價higheset_sale < lowest_purchase
        1. 結束並印出歸入權總和(profit)
    2. 如果highest_sale股數 < lowest_purchase 股數
        1. highest_sale股數 * highest_sale股價 - highest_sale股數 * lowest_purchase股價
        2. 計算current_profit，加入profit
        3. 更新purchase_data、sale_data
    3. 如果highest_sale股數 > lowest_purchase股數
        1. lowest_purchase股數 * highest_sale股價 - lowest_purchase股數 * lowest_purchase股價
        2. 計算current_profit，加入profit
        3. 更新purchase_data、sale_data
    4. 如果股數highest_sale = lowest_purchase
        1. highest_sale股數* highest_sale股價 - lowest_purchase股數 * lowest_purchase股價
        2. 計算current_profit，加入profit
        3. 更新purchase_data、sale_data

---

參考法條(以下皆為中華民國法律)：

證券交易法第157條

證券交易法施行細則第11條第二項

---

這是個不夠精緻，待完善的project，請多指教