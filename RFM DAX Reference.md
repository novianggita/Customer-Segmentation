# 📊 RFM DAX Reference

## 1. Sales After Discount
```dax
sales_after_discount = 'segmentation - superstore'[sales] - ('segmentation - superstore'[sales] * 'segmentation - superstore'[discount])
```

## 2. Initial RFM Table
```dax
RFM_Table = SUMMARIZE(
    'segmentation - superstore',
    'segmentation - superstore'[customer_id],
    'segmentation - superstore'[customer_name],
    "last_transaction", MAX('segmentation - superstore'[order_date]),
    "frequency", DISTINCTCOUNT('segmentation - superstore'[order_id]),
    "monetary", SUM('segmentation - superstore'[sales_after_discount])
)
```

## 3. Recency
``` dax
recency = DATEDIFF(RFM_Table[last_transaction], DATE(2017, 12, 31), DAY)
```

## 4. R Score
``` dax
r_score = SWITCH(
    TRUE(),
    RFM_Table[recency] <= PERCENTILE.INC(RFM_Table[recency], 0.20), "5",
    RFM_Table[recency] <= PERCENTILE.INC(RFM_Table[recency], 0.40), "4",
    RFM_Table[recency] <= PERCENTILE.INC(RFM_Table[recency], 0.60), "3",
    RFM_Table[recency] <= PERCENTILE.INC(RFM_Table[recency], 0.80), "2",
    "1"
)
```

## 5. F Score
``` dax
f_score = SWITCH(
    TRUE(),
    RFM_Table[frequency] >= PERCENTILE.INC(RFM_Table[frequency], 0.80), "5",
    RFM_Table[frequency] >= PERCENTILE.INC(RFM_Table[frequency], 0.60), "4",
    RFM_Table[frequency] >= PERCENTILE.INC(RFM_Table[frequency], 0.40), "3",
    RFM_Table[frequency] >= PERCENTILE.INC(RFM_Table[frequency], 0.20), "2",
    "1"
)
```

## 6. M Score
``` dax
m_score = SWITCH(
    TRUE(),
    RFM_Table[monetary] >= PERCENTILE.INC(RFM_Table[monetary], 0.80), "5",
    RFM_Table[monetary] >= PERCENTILE.INC(RFM_Table[monetary], 0.60), "4",
    RFM_Table[monetary] >= PERCENTILE.INC(RFM_Table[monetary], 0.40), "3",
    RFM_Table[monetary] >= PERCENTILE.INC(RFM_Table[monetary], 0.20), "2",
    "1"
)
```

## 7. RFM Score
``` dax
rfm_score = 'RFM_Table'[r_score] & 'RFM_Table'[f_score] & 'RFM_Table'[m_score]
```

## 8. Customer Segment
``` dax
customer_segment = 
SWITCH(
    TRUE(),
    'RFM_Table'[rfm_score] >= "511" && 'RFM_Table'[rfm_score] <= "555", "Champions",
    'RFM_Table'[rfm_score] >= "451" && 'RFM_Table'[rfm_score] <= "455", "Loyal",
    'RFM_Table'[rfm_score] >= "351" && 'RFM_Table'[rfm_score] <= "445", "Potential",
    'RFM_Table'[rfm_score] >= "211" && 'RFM_Table'[rfm_score] <= "345", "At Risk",
    "Need Attention"
)
```

## 9. Rank Customer by Recency
``` dax
rank_by_recency = RANKX(RFM_Table, RFM_Table[recency], , ASC)
```

## 10. Each Customer's Monetary Percentage of Total
``` dax
% monetary = RFM_Table[monetary] / sum(RFM_Table[monetary])
```

## 11. Rank Customer by Monetary
``` dax
rank_by_monetary = RANKX(RFM_Table, RFM_Table[monetary], , DESC)
```
