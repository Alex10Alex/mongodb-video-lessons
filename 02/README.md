Для импорта коллекции из blog.json востользуйтесь следующей командой

    mongoimport -dblog -cposts --jsonArray < blog.json

    newPost = {
        _id: 0,
        title: "Поддержим выпуск видео лекций по MongoDB",
        body: "Crud...",
        date: "2015-02-16T14:56:00.000Z",
        options: {isDraft: true, version: 1},
        tags: ["mongo", "video", "youtube", "lecture"]
    }

    db.posts.insert(newDoc)

    db.posts.find({_id: 0})

    db.posts.findOne({body: "Crud..."}, {title: true})

    db.posts.find({_id: {$gt: 5}}, {title: true})

    db.posts.find({_id: {$lt: 1}}, {title: true})

    db.posts.find({_id: {$lte: 1}}, {title: true})

    db.posts.find({_id: {$gt: 100}}).pretty()

    db.posts.find({title: {$gt: "П"}})

    db.posts.find({title: {$lt: "Г"}})

    db.posts.find({title: {$regex: /mongo/i}}, {title: true})

    db.posts.find({
        $or: [
            {title: {$regex: /mongo/i}},
            {title: {$regex: /dart/i}}
        ]},
        {title: true})

    db.posts.find({ $and :[ {body: {$regex: /mongo/i}}, {date: {$gt: "2015"}}] }).pretty()

    db.posts.find({
        body: {$regex: /mongo/i},
        date: {$gt: "2015"}
    }).pretty()

    db.posts.find({tags: {$in: ["mongo", "youtube"]}}, {tags: true})

    db.posts.find({tags: {$all: ["mongo", "youtube"]}}, {tags: true})

    db.posts.find({options: { "isDraft" : false, "author" : "Alex" }}, {options: true})

    db.posts.find({"options.version": 1}, {options: true})

    posts = db.posts.find({});null;

    while (posts.hasNext())posts.next()

    db.posts.find({}).sort({_id: -1}).skip(5).limit(2)