---
date : 2022-11-23 Wed 0:55
alias : []
---
---

這題和some assembly required1 相同, 可以去devtool中找wasm, 找到wasm之後用wget載下來, 將wasm用wasm2wat搭配`--generate-names`來對沒有wasm沒有名稱的type或function進行命名, 得到一個wat(wasm的文字檔案).但因為這樣在閱讀上還是有點困難, 可以將原始的wasm`使用wasm-decompile來產生一個pseudo code的dcmp檔.` 在dcmp檔的check_flag中可以看到, 有進行xor的動作, 且因為在some assembly 1中data的相同位置為flag, `猜測data部份可能被xor過`, 所以把這題的這個data部份拿去拿去跟picoCTF做xor會發現, 前方的值會得到重複的值x08, 依照xor的特性, 代表原字串是以8下去做xor, 因此將原data整串對8做xor後, 就可以得到flag.

在這題中嘗試使用的新工具有`wasm2wat`及`wasm-decompile`, 在處理wasm上都有其各自的方便性及不錯用參數.
原本也因為`xor`的觀念卡了一下, 也在解出flag之後再重新思考了一下xor的特性, 也對xor這個重要的運算方式更加理解了
