# 拆包和合包

```shell
#tar包
split -b 1024m oms.feature_2.0.0.202010232247.tar.gz oms.tar.gz.
cat oms.tar.gz.a* > oms.feature_2.0.0.202010232247.tar.gz
#zip包
zip -s 500m oms.zip --out oms.zip.
```

