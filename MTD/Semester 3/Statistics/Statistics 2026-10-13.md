#statistics 

![[02_R_Einleitung.pdf]]

``` R
df |> select(Month, Ozone, Temp) |> group_by(Month) |> summarise_all(mean)
```

