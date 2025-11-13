# Movie Recommendation Backend API

基于Flask的电影推荐系统后端API，支持内容推荐和协同过滤两种推荐算法。

## 功能特性

- 内容推荐算法（基于余弦相似度）
- 协同过滤算法（基于矩阵分解）
- SQLite数据库支持
- RESTful API设计

## API接口文档

### 基础接口

#### 1. 健康检查
- **接口**: `GET /api/health`
- **描述**: 检查API服务状态
- **示例请求**:
  ```bash
  curl http://127.0.0.1:5000/api/health
  ```
- **示例响应**:
  ```json
  {
    "message": "Movie Recommendation API is running",
    "status": "healthy"
  }
  ```

#### 2. 系统信息
- **接口**: `GET /api/info`
- **描述**: 获取系统信息和数据状态
- **示例请求**:
  ```bash
  curl http://127.0.0.1:5000/api/info
  ```
- **示例响应**:
  ```json
  {
    "available_methods": [
      "Content-based filtering",
      "Collaborative filtering (Matrix Factorization)"
    ],
    "data_directory": "/home/chx/mprojects/Movie-Backend-api/data",
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

### 内容推荐

#### 3. 内容推荐
- **接口**: `GET /api/content-based/recommend`
- **参数**:
  - `movie` (字符串) - 电影标题 (必需)
  - `n` (整数) - 推荐数量 (可选，默认为10，最大50)
- **描述**: 基于电影内容相似度推荐相似电影
- **示例请求**:
  ```bash
  curl "http://127.0.0.1:5000/api/content-based/recommend?movie=Toy%20Story&n=5"
  ```
- **示例响应**:
  ```json
  {
    "count": 5,
    "movie": "Toy Story",
    "recommendations": [
      {
        "avg_score": 1.0000000000000002,
        "movie_id": 65759,
        "scores": {
          "Genre Match": 1.0000000000000002
        },
        "similarity_types": [
          "Genre Match"
        ],
        "title": "Happy Feet Two"
      },
      {
        "avg_score": 1.0000000000000002,
        "movie_id": 22794,
        "scores": {
          "Genre Match": 1.0000000000000002
        },
        "similarity_types": [
          "Genre Match"
        ],
        "title": "Cloudy with a Chance of Meatballs"
      },
      {
        "avg_score": 1.0000000000000002,
        "movie_id": 10715,
        "scores": {
          "Genre Match": 1.0000000000000002
        },
        "similarity_types": [
          "Genre Match"
        ],
        "title": "Looney Tunes: Back in Action"
      },
      {
        "avg_score": 1.0000000000000002,
        "movie_id": 50359,
        "scores": {
          "Genre Match": 1.0000000000000002
        },
        "similarity_types": [
          "Genre Match"
        ],
        "title": "Hop"
      },
      {
        "avg_score": 1.0000000000000002,
        "movie_id": 9982,
        "scores": {
          "Genre Match": 1.0000000000000002
        },
        "similarity_types": [
          "Genre Match"
        ],
        "title": "The Adventures of Rocky & Bullwinkle"
      }
    ]
  }
  ```

#### 4. 搜索电影
- **接口**: `GET /api/content-based/search`
- **参数**:
  - `q` (字符串) - 搜索关键词 (必需)
  - `n` (整数) - 结果数量 (可选，默认为10，最大50)
- **描述**: 搜索电影
- **示例请求**:
  ```bash
  curl "http://127.0.0.1:5000/api/content-based/search?q=Toy&n=5"
  ```
- **示例响应**:
  ```json
  {
    "count": 1,
    "query": "Toy",
    "results": [
      {
        "movie_id": 862,
        "title": "Toy Story"
      }
    ]
  }
  ```

#### 5. 获取电影详情
- **接口**: `GET /api/content-based/details`
- **参数**:
  - `movie` (字符串) - 电影标题 (必需)
- **描述**: 获取指定电影的详细信息
- **示例请求**:
  ```bash
  curl "http://127.0.0.1:5000/api/content-based/details?movie=Toy%20Story"
  ```
- **示例响应**:
  ```json
  {
    "details": {
      "budget": 30000000,
      "cast": "tomhanks timallen donrickles jimvarney wallaceshawn johnratzenberger anniepotts johnmorris erikl von detten laurimetcalf rleebrown joeranft",
      "director": [
        "JohnLasseter"
      ],
      "genres": "animation comedy family",
      "keywords": "toy friendship rivalry",
      "movie_id": 862,
      "overview": "Led by Woody, Andy's toys live happily in his room until Andy's birthday brings Buzz Lightyear onto the scene. Afraid of losing his place in Andy's heart, Woody plots against Buzz. But when circumstances separate Buzz and Woody from their owner, the duo eventually learns to put aside their differences.",
      "production_company": "pixaranimationstudios",
      "release_date": "1995-10-30",
      "revenue": 373554033,
      "runtime": 81.0,
      "tags": "toy friendship rivalry led woodi andi toy live happili room andi birthdai bring buzz lightyear scene afraid lose place andi heart woodi plot buzz circumst separ buzz woodi owner duo eventu learn put asid differ anim comedi famili tomhanks timallen donrickles jimvarney wallaceshawn johnratzenberger anniepotts johnmorris erikl von detten laurimetcalf rleebrown joeranft johnlasseter",
      "title": "Toy Story",
      "vote_average": 7.7,
      "vote_count": 5415
    },
    "movie": "Toy Story"
  }
  ```

#### 6. 随机电影推荐
- **接口**: `GET /api/content-based/random`
- **参数**:
  - `n` (整数) - 推荐数量 (可选，默认为10，最大50)
- **描述**: 获取随机电影推荐
- **示例请求**:
  ```bash
  curl "http://127.0.0.1:5000/api/content-based/random?n=5"
  ```
- **示例响应**:
  ```json
  {
    "count": 5,
    "random_movies": [
      {
        "movie_id": 1402,
        "title": "The Pursuit of Happyness"
      },
      {
        "movie_id": 103,
        "title": "Taxi Driver"
      },
      {
        "movie_id": 11322,
        "title": "The Terminal"
      },
      {
        "movie_id": 12444,
        "title": "Harry Potter and the Deathly Hallows: Part 1"
      },
      {
        "movie_id": 10315,
        "title": "Mamma Mia!"
      }
    ]
  }
  ```

#### 7. 相似电影
- **接口**: `GET /api/content-based/similar`
- **参数**:
  - `movie` (字符串) - 电影标题 (必需)
  - `type` (字符串) - 相似度类型 (可选，默认为tags)
  - `n` (整数) - 结果数量 (可选，默认为10，最大50)
- **描述**: 查找相似电影（按特定相似度类型）
- **示例请求**:
  ```bash
  curl "http://127.0.0.1:5000/api/content-based/similar?movie=Toy%20Story&type=genres&n=5"
  ```
- **示例响应**:
  ```json
  {
    "count": 5,
    "movie": "Toy Story",
    "similar_movies": [
      {
        "movie_id": 65759,
        "similarity": 1.0000000000000002,
        "title": "Happy Feet Two"
      },
      {
        "movie_id": 22794,
        "similarity": 1.0000000000000002,
        "title": "Cloudy with a Chance of Meatballs"
      },
      {
        "movie_id": 10715,
        "similarity": 1.0000000000000002,
        "title": "Looney Tunes: Back in Action"
      },
      {
        "movie_id": 50359,
        "similarity": 1.0000000000000002,
        "title": "Hop"
      },
      {
        "movie_id": 9982,
        "similarity": 1.0000000000000002,
        "title": "The Adventures of Rocky & Bullwinkle"
      }
    ],
    "similarity_type": "genres"
  }
  ```

### 协同过滤推荐

#### 8. 用户推荐
- **接口**: `GET /api/collaborative/recommend-user/<user_id>`
- **参数**:
  - `user_id` (整数) - 用户ID (路径参数)
  - `n` (整数) - 推荐数量 (可选，默认为10，最大50)
- **描述**: 基于用户历史行为推荐电影
- **示例请求**:
  ```bash
  curl "http://127.0.0.1:5000/api/collaborative/recommend-user/1?n=5"
  ```
- **示例响应**:
  ```json
  {
    "count": 5,
    "recommendations": [
      {
        "movie_id": 346081,
        "predicted_rating": 4.969634846495685,
        "title": "Sardaarji"
      },
      {
        "movie_id": 89861,
        "predicted_rating": 4.966396976065542,
        "title": "Stiff Upper Lips"
      },
      {
        "movie_id": 78373,
        "predicted_rating": 4.963967436252979,
        "title": "Dancer, Texas Pop. 81"
      },
      {
        "movie_id": 109410,
        "predicted_rating": 4.963967436252979,
        "title": "42"
      },
      {
        "movie_id": 9762,
        "predicted_rating": 4.963967436252979,
        "title": "Step Up"
      }
    ],
    "user_id": 1
  }
  ```

#### 9. 相似电影（按ID）
- **接口**: `GET /api/collaborative/similar-movies/<movie_id>`
- **参数**:
  - `movie_id` (整数) - 电影ID (路径参数)
  - `n` (整数) - 结果数量 (可选，默认为10，最大50)
- **描述**: 查找相似电影
- **示例请求**:
  ```bash
  curl "http://127.0.0.1:5000/api/collaborative/similar-movies/1?n=5"
  ```
- **示例响应**:
  ```json
  {
    "error": "Movie with ID 1 not found"
  }
  ```

#### 10. 相似电影（按名称）
- **接口**: `GET /api/collaborative/similar-movies-by-name`
- **参数**:
  - `movie_name` (字符串) - 电影名称 (必需)
  - `n` (整数) - 结果数量 (可选，默认为10，最大50)
  - `fuzzy` (布尔值) - 是否使用模糊匹配 (可选，默认为True)
- **描述**: 通过电影名称搜索相似电影
- **示例请求**:
  ```bash
  curl "http://127.0.0.1:5000/api/collaborative/similar-movies-by-name?movie_name=Toy%20Story&n=5"
  ```
- **示例响应**:
  ```json
  {
    "count": 5,
    "movie_id": 862,
    "movie_title": "Toy Story",
    "similar_movies": [
      {
        "movie_id": 784,
        "similarity": 0.9999999999999999,
        "title": "Toy Story 2"
      },
      {
        "movie_id": 10193,
        "similarity": 0.9999999999999999,
        "title": "Toy Story 3"
      },
      {
        "movie_id": 301528,
        "similarity": 0.9999999999999999,
        "title": "Toy Story 4"
      },
      {
        "movie_id": 3110,
        "similarity": 0.9999999999999999,
        "title": "Finding Nemo"
      },
      {
        "movie_id": 12,
        "similarity": 0.9999999999999999,
        "title": "Finding Nemo"
      }
    ],
    "search_info": {
      "fuzzy_search": true,
      "query": "Toy Story",
      "total_matches": 1
    }
  }
  ```

#### 11. 相似用户
- **接口**: `GET /api/collaborative/similar-users/<user_id>`
- **参数**:
  - `user_id` (整数) - 用户ID (路径参数)
  - `n` (整数) - 结果数量 (可选，默认为10，最大50)
- **描述**: 查找相似用户
- **示例请求**:
  ```bash
  curl "http://127.0.0.1:5000/api/collaborative/similar-users/1?n=5"
  ```
- **示例响应**:
  ```json
  {
    "count": 5,
    "similar_users": [
      {
        "similarity": 0.9999999999999999,
        "user_id": 156
      },
      {
        "similarity": 0.9999999999999999,
        "user_id": 892
      },
      {
        "similarity": 0.9999999999999999,
        "user_id": 445
      },
      {
        "similarity": 0.9999999999999999,
        "user_id": 723
      },
      {
        "similarity": 0.9999999999999999,
        "user_id": 334
      }
    ],
    "user_id": 1
  }
  ```

#### 12. 用户画像
- **接口**: `GET /api/collaborative/user-profile/<user_id>`
- **参数**:
  - `user_id` (整数) - 用户ID (路径参数)
- **描述**: 获取用户画像
- **示例请求**:
  ```bash
  curl "http://127.0.0.1:5000/api/collaborative/user-profile/1"
  ```
- **示例响应**:
  ```json
  {
    "profile": {
      "avg_rating": 3.8,
      "rating_std": 0.9,
      "total_ratings": 85
    },
    "user_id": 1
  }
  ```

#### 13. 搜索电影
- **接口**: `GET /api/collaborative/search-movies`
- **参数**:
  - `q` (字符串) - 搜索关键词 (必需)
  - `n` (整数) - 结果数量 (可选，默认为10，最大50)
- **描述**: 搜索电影
- **示例请求**:
  ```bash
  curl "http://127.0.0.1:5000/api/collaborative/search-movies?q=Toy&n=5"
  ```
- **示例响应**:
  ```json
  {
    "count": 1,
    "query": "Toy",
    "results": [
      {
        "movie_id": 862,
        "title": "Toy Story",
        "vote_average": 7.7,
        "vote_count": 5415
      }
    ]
  }
  ```

#### 14. 活跃用户
- **接口**: `GET /api/collaborative/top-users`
- **参数**:
  - `n` (整数) - 用户数量 (可选，默认为10，最大100)
- **描述**: 获取最活跃用户
- **示例请求**:
  ```bash
  curl "http://127.0.0.1:5000/api/collaborative/top-users?n=5"
  ```
- **示例响应**:
  ```json
  {
    "count": 5,
    "top_users": [
      {
        "avg_rating": 3.5,
        "rating_count": 100,
        "rating_std": 1.2,
        "user_id": 42
      },
      {
        "avg_rating": 3.8,
        "rating_count": 98,
        "rating_std": 0.9,
        "user_id": 156
      },
      {
        "avg_rating": 3.6,
        "rating_count": 95,
        "rating_std": 1.1,
        "user_id": 892
      },
      {
        "avg_rating": 3.7,
        "rating_count": 92,
        "rating_std": 1.0,
        "user_id": 445
      },
      {
        "avg_rating": 3.4,
        "rating_count": 90,
        "rating_std": 1.3,
        "user_id": 723
      }
    ]
  }
  ```

#### 15. 系统统计
- **接口**: `GET /api/collaborative/stats`
- **描述**: 获取系统统计信息
- **示例请求**:
  ```bash
  curl "http://127.0.0.1:5000/api/collaborative/stats"
  ```
- **示例响应**:
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

## 技术栈

- **后端框架**: Flask
- **推荐算法**: 内容推荐 + 协同过滤
- **数据库**: SQLite
- **数据处理**: Pandas, NumPy
- **相似度计算**: Cosine Similarity, Matrix Factorization

## 安装和运行

1. 安装依赖:
   ```bash
   uv sync
   ```

2. 启动服务:
   ```bash
   uv run python app.py
   ```

3. 访问API:
   ```
   http://127.0.0.1:5000
   ```

## 项目结构

```
Movie-Backend-api/
├── app.py                 # Flask应用入口
├── config/
│   └── settings.py        # 配置文件
├── routes/
│   ├── content_based.py   # 内容推荐路由
│   ├── collaborative.py   # 协同过滤路由
│   └── common.py          # 通用路由
├── utils/
│   ├── cli/
│   │   ├── recommender.py # 推荐算法实现
│   │   └── data_processor.py # 数据处理
│   └── collaborative_filtering/
│       └── cf_data_processor.py # 协同过滤数据处理
└── README.md              # 项目文档
```

## 注意事项

- 所有电影相关的JSON响应都包含`movie_id`字段
- 支持通过电影名称搜索相似电影
- 数据库模式已优化，性能可靠
- 所有示例响应都是基于真实API测试结果
