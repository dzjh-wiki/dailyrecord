# Django的用法

## 1. 安装
```
pip install Django
```

## 2. 创建项目
```
django-admin startproject ProjectName
```

## 3. 运行项目
  * 进入 ProjectName 目录，运行一下命令：
```
python manage.py runserver 127.0.0.1:8000
```

## 4. 创建模型
```
django-admin startapp ModelName
```

## 5. 反向生成models
```
python manage.py inspectdb
```