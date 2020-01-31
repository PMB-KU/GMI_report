# Report about bam file from GMI

ツールが正常に動いているかどうかは、SRR9974606を用いて確認した。

## GMIのbamファイルは正常なbamファイルではない

まず、bam -> bigwig変換をdeeptoolsを使って試みた。

```bash
for file in `ls *.bam`; do
    samtools sort 
    samtools index {$file%.bam}_sorted.bam
    bamCoverage --bam $file -o ${file%.bam}.bw -of --bigwig
done
```

SRR9974606では正常にbigwigに変換することができたことから、

以下のエラーが起きたので、BAMファイルのフォーマットが異常であると考えられた。

そこで、samtoolsを用いてBAMの中身を確認した。

```bash
for file in `ls *.bam`; do 
    samtools view $file | head -n 100 > $file.n100.txt
done
```

その結果、正常なBAMと異なり、リードの位置情報などが失われていた。
