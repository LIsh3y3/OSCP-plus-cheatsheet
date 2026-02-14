すこし理解に時間がかかった文法をまとめる。

---

# Mainについて




---

# 文字列操作

## 文字列の置き換え

```python
print 'My name is %s.' % ('Ikeda')
# My name is Ikeda.
```
```python
# exploit.py http://examle
url = sys.argv[1]
url_expliot= "%s/../../../etc/passwd" % (url)
print url_exploit
# http://example/../../etc/passwd
```

