title: "golang strcut 初始化"
date: 2020-03-14 18:04:57
tags: "golang"
---

#### 说明
> 刚刚接触golang, 准备用golang gin mongodb写一个CRUD小Demo，把遇到的知识点记录下来, 给自己以后参考

#### 定义Post struct

```
# module/post.go
type Post struct {
    ID       primitive.ObjectID `bson:"_id,omitempty" json:"id,omitempty"`
    Title    string             `bson:"title,omitempty" json:"title,omitempty"`
    Content  string             `bson:"content,omitempty" json:"content,omitempty"`
    Type     int                `bson:"type,omitempty" json:"type,omitempty"`
    Ctime    int64              `bson:"ctime,omitempty" json:"ctime,omitempty"`
    Utime    int64              `bson:"utime,omitempty" json:"utime,omitempty"`
    UserID   int                `bson:"user_id,omitempty" json:"user_id,omitempty"`
    ReferURL string             `bson:"refer_url,omitempty" json:"refer_url,omitempty"`
    Extra    string             `bson:"extra,omitempty" json:"extra,omitempty"`
}
```

#### 初始化

```
# 方法一
post := models.Post{primitive.NewObjectID(), "title", "content", 1, time.Now().Unix(), time.Now().Unix(), 1, "http://www.baidu.com", ""}
models.CreatePost(post)

# 方法二
var post models.Post
post.ID = primitive.NewObjectID()
post.Title = "这是一个title4"
post.Content = "这是content，有长度"
post.Type = 1
post.Ctime = time.Now().Unix()
post.Utime = time.Now().Unix()
post.UserID = 1
post.ReferURL = "http://www.baidu.com"
post.Extra = ""

models.CreatePost(post)
```
