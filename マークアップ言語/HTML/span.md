### spanとは

使い方としてはdivと同じで効果範囲を指定するのに使われる。spanは英語で「長さ」の意味。

### divとの違い

divとの違いはdivが`display:block`であるのに対し、spanは`diaplay:inline`であるという違いがある。

なので、spanは**文章の途中で文字の色を変えたり、下線を引いたりする時に使われる。**

**具体例**

```
<p>俺は<span class="red">世界最強</span>の男だ</p>
```

```
.red {
   color:red;
   font-weight:bold;
}
```
