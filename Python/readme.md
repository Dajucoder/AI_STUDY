# 1、虚拟环境
## 1、使用 py 启动器指定 Python 版本

```bash
py -3.11 -m venv venv
```

## 2、激活虚拟环境
```bash
.\venv\Scripts\activate
```

## 3、安装依赖
```bash
pip install -r requirements.txt
```

## 4、指定版本安装依赖
```bash
pip install -r requirements.txt --upgrade
```

## 5、生成依赖文件
```bash
pip freeze > requirements.txt   #利用 `pip` 包管理器的功能，将当前 Python 环境中已安装的包及其版本信息导出到一个名为 `requirements.txt` 的文件中。
```

## 6、注意事项
- requirements.txt 文件必须使用UTF-8编码保存
- 确保没有BOM标记（UTF-8无BOM）
- 所有注释使用`#`符号，不要使用`//`
```bash
# 使用-E参数指定编码
pip install -r requirements.txt -E utf-8
```


