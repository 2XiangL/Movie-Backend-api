# 快速开始

For uv users:

```sh
uv venv
uv sync
uv run app.py
```

For conda users:

```sh
conda create flask python=3.11
conda activate flask
pip install -r requirements.txt
python3 app.py
```

Or:

```sh
python -m venv .venv
source ./.venv/bin/activate
pip3 install -r requirements.txt
python3 app.py
```

注意：你需要把data目录解压到项目根目录下

## API接口文档

提供内容推荐和协同过滤两种推荐算法。

## 健康检查

### GET /health
健康检查接口

**响应示例：**
```json
{
  "status": "healthy",
  "message": "Movie Recommendation API is running"
}
```

## 通用接口

### GET /api/info
获取系统信息

**响应示例：**
```json
{
    "available_methods": [
        "Content-based filtering",
        "Collaborative filtering (Matrix Factorization)"
    ],
    "data_directory": "/home/chx/mprojects/flask-movie-api/data",
    "data_status": {
        "cf_model": "True",
        "credits_csv": "True",
        "movies_csv": "True",
        "similarity_matrices": 5
    },
    "endpoints": {
        "collaborative": "/api/collaborative/*",
        "common": "/api/*",
        "content_based": "/api/content-based/*"
    },
    "system": "Movie Recommendation API",
    "version": "1.0.0"
}
```

## 内容推荐接口

### GET /api/content-based/recommend
基于内容的电影推荐

**参数：**
- `movie` (必需): 电影标题
- `n` (可选): 推荐数量，默认为10，最大50

**响应示例：**
```json
{
    "count": 10,
    "movie": "Avatar",
    "recommendations": [
        {
            "avg_score": 1.0,
            "scores": {
                "Genre Match": 1.0
            },
            "similarity_types": [
                "Genre Match"
            ],
            "title": "Superman"
        },
        {
            "avg_score": 0.8944271909999159,
            "scores": {
                "Genre Match": 0.8944271909999159
            },
            "similarity_types": [
                "Genre Match"
            ],
            "title": "Teenage Mutant Ninja Turtles"
        },
        {
            "avg_score": 0.8944271909999159,
            "scores": {
                "Genre Match": 0.8944271909999159
            },
            "similarity_types": [
                "Genre Match"
            ],
            "title": "Mystery Men"
        },
        {
            "avg_score": 0.8944271909999159,
            "scores": {
                "Genre Match": 0.8944271909999159
            },
            "similarity_types": [
                "Genre Match"
            ],
            "title": "The Shadow"
        },
        {
            "avg_score": 0.8660254037844388,
            "scores": {
                "Genre Match": 0.8660254037844388
            },
            "similarity_types": [
                "Genre Match"
            ],
            "title": "Pirates of the Caribbean: At World's End"
        },
        {
            "avg_score": 0.8660254037844388,
            "scores": {
                "Genre Match": 0.8660254037844388
            },
            "similarity_types": [
                "Genre Match"
            ],
            "title": "Pirates of the Caribbean: Dead Man's Chest"
        },
        {
            "avg_score": 0.8660254037844388,
            "scores": {
                "Genre Match": 0.8660254037844388
            },
            "similarity_types": [
                "Genre Match"
            ],
            "title": "The Avengers"
        },
        {
            "avg_score": 0.8660254037844388,
            "scores": {
                "Genre Match": 0.8660254037844388
            },
            "similarity_types": [
                "Genre Match"
            ],
            "title": "The Hobbit: The Battle of the Five Armies"
        },
        {
            "avg_score": 0.8660254037844388,
            "scores": {
                "Genre Match": 0.8660254037844388
            },
            "similarity_types": [
                "Genre Match"
            ],
            "title": "The Amazing Spider-Man"
        },
        {
            "avg_score": 0.8660254037844388,
            "scores": {
                "Genre Match": 0.8660254037844388
            },
            "similarity_types": [
                "Genre Match"
            ],
            "title": "Spider-Man 2"
        }
    ]
}
```

### GET /api/content-based/search
搜索电影

**参数：**
- `q` (必需): 搜索关键词
- `n` (可选): 结果数量，默认为10，最大50

**响应示例：**
```json
{
    "count": 1,
    "query": "avatar",
    "results": [
        "Avatar"
    ]
}
```

### GET /api/content-based/details
获取电影详细信息

**参数：**
- `movie` (必需): 电影标题

**响应示例：**
```json
{
    "details": {
        "budget": 237000000,
        "cast": "samworthington zoesaldana sigourneyweaver stephenlang michellerodriguez giovanniribisi joeldavidmoore cchpounder wesstudi lazalonso",
        "director": [
            "JamesCameron"
        ],
        "genres": "action adventure fantasy sciencefiction",
        "keywords": "cultureclash futur spacewar spacecoloni societi spacetravel futurist romanc space alien tribe alienplanet cgi marin soldier battl loveaffair antiwar powerrel mindandsoul",
        "movie_id": 19995,
        "overview": "In the 22nd century, a paraplegic Marine is dispatched to the moon Pandora on a unique mission, but becomes torn between following orders and protecting an alien civilization.",
        "production_company": "ingeniousfilmpartners twentiethcenturyfoxfilmcorporation duneentertainment lightstormentertainment",
        "release_date": "2009-12-10",
        "revenue": 2787965087,
        "runtime": 162.0,
        "tags": "22nd century parapleg marin dispatch moon pandora uniqu mission becom torn follow order protect alien civilization action adventur fantasi sciencefict cultureclash futur spacewar spacecoloni societi spacetravel futurist romanc space alien tribe alienplanet cgi marin soldier battl loveaffair antiwar powerrel mindandsoul samworthington zoesaldana sigourneyweav stephenlang michellerodriguez giovanniribisi joeldavidmoor cchpounder wesstudi lazalonso jamescameron",
        "title": "Avatar",
        "vote_average": 7.2,
        "vote_count": 11800
    },
    "movie": "avatar"
}
```

### GET /api/content-based/random
获取随机电影推荐

**参数：**
- `n` (可选): 随机电影数量，默认为10，最大50

**响应示例：**
```json
{
    "count": 10,
    "random_movies": [
        "Amidst the Devil's Wings",
        "Sharknado",
        "Bad Grandpa",
        "Battle for the Planet of the Apes",
        "They Came Together",
        "Time to Choose",
        "Date Night",
        "Deceptive Practice: The Mysteries and Mentors of Ricky Jay",
        "Warrior",
        "Prophecy"
    ]
}
```

### GET /api/content-based/similar
查找相似电影

**参数：**
- `movie` (必需): 电影标题
- `type` (可选): 相似度类型，默认为tags，可选值: tags, genres, keywords, cast, production
- `n` (可选): 结果数量，默认为10，最大50

**响应示例：**
```json
{
    "count": 10,
    "movie": "avatar",
    "similar_movies": [
        [
            "Aliens vs Predator: Requiem",
            0.2665300196340371
        ],
        [
            "Independence Day",
            0.24602771043141894
        ],
        [
            "Aliens",
            0.23870495801314429
        ],
        [
            "Titan A.E.",
            0.2369241569216954
        ],
        [
            "Meet Dave",
            0.21995293510729183
        ],
        [
            "E.T. the Extra-Terrestrial",
            0.21995293510729183
        ],
        [
            "Small Soldiers",
            0.21982600255746473
        ],
        [
            "Attack the Block",
            0.21982600255746473
        ],
        [
            "Lifeforce",
            0.21977383072747692
        ],
        [
            "Predators",
            0.2172620473133704
        ]
    ],
    "similarity_type": "tags"
}
```

## 协同过滤接口

### GET /api/collaborative/similar-movies/<int:movie_id>
查找相似电影

**参数：**
- `n` (可选): 相似电影数量，默认为10，最大50

**响应示例：**
```json
{
    "count": 10,
    "movie_id": 19995,
    "movie_title": "Avatar",
    "similar_movies": [
        {
            "movie_id": 462,
            "similarity": 0.7327219331356214,
            "title": "Erin Brockovich"
        },
        {
            "movie_id": 21494,
            "similarity": 0.6543455590854379,
            "title": "Moliere"
        },
        {
            "movie_id": 24206,
            "similarity": 0.5500205445450306,
            "title": "Silent Trigger"
        },
        {
            "movie_id": 10288,
            "similarity": 0.5375343529563125,
            "title": "Fido"
        },
        {
            "movie_id": 17577,
            "similarity": 0.5164907231326318,
            "title": "The Devil's Tomb"
        },
        {
            "movie_id": 13518,
            "similarity": 0.5116424775475181,
            "title": "Fuel"
        },
        {
            "movie_id": 771,
            "similarity": 0.508061909263024,
            "title": "Home Alone"
        },
        {
            "movie_id": 72113,
            "similarity": 0.5067878430480488,
            "title": "Carnage"
        },
        {
            "movie_id": 12184,
            "similarity": 0.504184400329522,
            "title": "The Other Boleyn Girl"
        },
        {
            "movie_id": 11228,
            "similarity": 0.49978211198181155,
            "title": "Daylight"
        }
    ]
}
```

### GET /api/collaborative/search-movies
搜索电影

**参数：**
- `q` (必需): 搜索关键词
- `n` (可选): 结果数量，默认为10，最大50

**响应示例：**
```json
{
  "query": "avatar",
  "results": [
    {
      "movie_id": 19995,
      "title": "Avatar",
      "vote_average": 7.2,
      "vote_count": 22400
    }
  ],
  "count": 1
}
```

### GET /api/collaborative/stats
获取系统统计信息

**响应示例：**
```json
{
    "basic_stats": {
        "movies_count": 4803,
        "rating_density": 1.25,
        "ratings_count": 60275,
        "users_count": 1000
    },
    "model_config": {
        "learning_rate": 0.01,
        "n_epochs": 50,
        "n_factors": 30,
        "regularization": 0.1
    },
    "movie_popularity": {
        "avg_ratings_per_movie": 12.7,
        "max_ratings_per_movie": 57,
        "min_ratings_per_movie": 1
    },
    "rating_distribution": [
        {
            "count": 22,
            "percentage": 0.0,
            "rating": 0.5
        },
        {
            "count": 8,
            "percentage": 0.0,
            "rating": 0.6
        },
        {
            "count": 7,
            "percentage": 0.0,
            "rating": 0.7
        },
        {
            "count": 14,
            "percentage": 0.0,
            "rating": 0.8
        },
        {
            "count": 16,
            "percentage": 0.0,
            "rating": 0.9
        },
        {
            "count": 15,
            "percentage": 0.0,
            "rating": 1.0
        },
        {
            "count": 13,
            "percentage": 0.0,
            "rating": 1.1
        },
        {
            "count": 14,
            "percentage": 0.0,
            "rating": 1.2
        },
        {
            "count": 18,
            "percentage": 0.0,
            "rating": 1.3
        },
        {
            "count": 14,
            "percentage": 0.0,
            "rating": 1.4
        },
        {
            "count": 15,
            "percentage": 0.0,
            "rating": 1.5
        },
        {
            "count": 19,
            "percentage": 0.0,
            "rating": 1.6
        },
        {
            "count": 15,
            "percentage": 0.0,
            "rating": 1.7
        },
        {
            "count": 22,
            "percentage": 0.0,
            "rating": 1.8
        },
        {
            "count": 33,
            "percentage": 0.1,
            "rating": 1.9
        },
        {
            "count": 51,
            "percentage": 0.1,
            "rating": 2.0
        },
        {
            "count": 47,
            "percentage": 0.1,
            "rating": 2.1
        },
        {
            "count": 83,
            "percentage": 0.1,
            "rating": 2.2
        },
        {
            "count": 90,
            "percentage": 0.1,
            "rating": 2.3
        },
        {
            "count": 135,
            "percentage": 0.2,
            "rating": 2.4
        },
        {
            "count": 206,
            "percentage": 0.3,
            "rating": 2.5
        },
        {
            "count": 195,
            "percentage": 0.3,
            "rating": 2.6
        },
        {
            "count": 321,
            "percentage": 0.5,
            "rating": 2.7
        },
        {
            "count": 392,
            "percentage": 0.7,
            "rating": 2.8
        },
        {
            "count": 539,
            "percentage": 0.9,
            "rating": 2.9
        },
        {
            "count": 655,
            "percentage": 1.1,
            "rating": 3.0
        },
        {
            "count": 914,
            "percentage": 1.5,
            "rating": 3.1
        },
        {
            "count": 1103,
            "percentage": 1.8,
            "rating": 3.2
        },
        {
            "count": 1439,
            "percentage": 2.4,
            "rating": 3.3
        },
        {
            "count": 1659,
            "percentage": 2.8,
            "rating": 3.4
        },
        {
            "count": 2121,
            "percentage": 3.5,
            "rating": 3.5
        },
        {
            "count": 2405,
            "percentage": 4.0,
            "rating": 3.6
        },
        {
            "count": 2800,
            "percentage": 4.6,
            "rating": 3.7
        },
        {
            "count": 3081,
            "percentage": 5.1,
            "rating": 3.8
        },
        {
            "count": 3439,
            "percentage": 5.7,
            "rating": 3.9
        },
        {
            "count": 3749,
            "percentage": 6.2,
            "rating": 4.0
        },
        {
            "count": 3728,
            "percentage": 6.2,
            "rating": 4.1
        },
        {
            "count": 3864,
            "percentage": 6.4,
            "rating": 4.2
        },
        {
            "count": 3797,
            "percentage": 6.3,
            "rating": 4.3
        },
        {
            "count": 3644,
            "percentage": 6.0,
            "rating": 4.4
        },
        {
            "count": 3427,
            "percentage": 5.7,
            "rating": 4.5
        },
        {
            "count": 3099,
            "percentage": 5.1,
            "rating": 4.6
        },
        {
            "count": 2771,
            "percentage": 4.6,
            "rating": 4.7
        },
        {
            "count": 2357,
            "percentage": 3.9,
            "rating": 4.8
        },
        {
            "count": 1988,
            "percentage": 3.3,
            "rating": 4.9
        },
        {
            "count": 5931,
            "percentage": 9.8,
            "rating": 5.0
        }
    ],
    "user_activity": {
        "avg_ratings_per_user": 60.3,
        "max_ratings_per_user": 100,
        "min_ratings_per_user": 20
    }
}
```
