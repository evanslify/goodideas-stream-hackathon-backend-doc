# 好想直撥挑戰

認證方式: Header `Authorization: OAuth <fb_access_token>`


註：若返回 `400`，有可能會帶有某些錯誤訊息，格式未定

# Group 基礎

## 登入 [POST /login]

- Response 200
    - Attributes (Success)
        - `data`
            - `profile` (BuyerInfo)
            - `active_stream_id`: `f0185907-9b22-4f6b-8593-cf0837f80b45` (string, nullable) - 目前開的直播 ID，若無則 null
            - `has_active_stream`: `false` (boolean) - 是否有直播

# Group 買方資訊
## 買方資訊 [/profile]
### 修改 [PUT]

- Request (application/json)
    - Attributes (BuyerInfo)

- Response 200
    - Attributes (Success)
- Response 400
    - Attributes (Failure)

### 取得 [GET]

- Response 200 (application/json)
    - Attributes (Success)
        - `data` (BuyerInfo)

# Group 直播賣場操作
## 列出直播賣場 [GET /stream]

- Response 200 (application/json)
    - Attributes (Success)
        - `data` (array)
            - (object)
                - `id`: `f0185907-9b22-4f6b-8593-cf0837f80b45`
                - `url`: `http://example.com`
                - `name`: `直播 A`
            - (object)
                - `id`: `cf58e62c-3861-4082-97f3-07081012efa6`
                - `url`: `http://example.com`
                - `name`: `直播 B`
            - (object)
                - `id`: `df58e62c-3861-4082-97f3-07081012efa6`
                - `url`: `http://example.com`
                - `name`: `直播 C`

## 新增直播賣場 [POST /stream]

- Request (application/json)
    - Attributes
        - `url`: `http://example.com`
        - `name`: `直播 A`

- Response 200 (application/json)
    - Attributes (Success)
        - `data`
            - `id`: `f0185907-9b22-4f6b-8593-cf0837f80b45`

## 關閉直播賣場 [POST /stream/{stream-id}/close]

只有賣場擁有人可以呼叫。

- Parameters
    - `stream-id`: `f0185907-9b22-4f6b-8593-cf0837f80b45` - 直播 ID

- Response 200
    - Attributes (Success)
- Response 404
    - Attributes (Failure)

### 取得產品列表 [GET /stream/{stream-id}/product]

- Parameters
    - `stream-id`: `f0185907-9b22-4f6b-8593-cf0837f80b45` - 直播 ID

- Response 200
    - Attributes (Success)
        - `data` (array, fixed)
            - (object)
                - `quantity`: `1` (number) - 賣出數量
                - `title`: `商品1`
                - `price`: `100` (number)
                - `created_at`: `2018-01-01T00:00:00.000Z`
                - `image_url`: `https://fakeimg.pl/200x200`
                - `id`: `1` (number) - 項次

- Response 404
    - Attributes (Failure)

### 取得產品買家列表 [GET /stream/{stream-id}/product/{product-id}/buyers]

- Parameters
    - `stream-id`: `f0185907-9b22-4f6b-8593-cf0837f80b45` - 直播 ID
    - `product-id`: `a0185907-9b22-4f6b-8593-cf0837f80b45` - 商品 ID

- Response 200
    - Attributes (Success)
        - `data` (array, fixed)
            - (object)
                - `buyer` (BuyerInfo)
                - `quantity`: `1` (number)

- Response 404
    - Attributes (Failure)

#### 新增產品 [POST /stream/{stream-id}/product]

只有賣場擁有人可以呼叫。

- Parameters
    - `stream-id`: `f0185907-9b22-4f6b-8593-cf0837f80b45` - 直播 ID

- Request (application/json)
    - Attributes (Product)

- Response 200
    - Attributes (Success)
        - `data`
            - `id`: `a0185907-9b22-4f6b-8593-cf0837f80b45` - 直播 ID
- Response 400
    - Attributes (Failure)

#### 修改產品 [PUT /stream/{stream-id}/product/{product-id}]

只有賣場擁有人可以呼叫。

- Parameters
    - `stream-id`: `f0185907-9b22-4f6b-8593-cf0837f80b45` - 直播 ID
    - `product-id`: `1` - 產品項次

- Request (application/json)
    - Attributes (Product)

- Response 200
    - Attributes (Success)
- Response 400
    - Attributes (Failure)

#### 移除產品 [DELETE /stream/{stream-id}/product/{product-id}]

只有賣場擁有人可以呼叫。

- Parameters
    - `stream-id`: `a0185907-9b22-4f6b-8593-cf0837f80b45` - 直播 ID
    - `product-id`: `1` - 產品項次

- Response 200
    - Attributes (Success)
- Response 404
    - Attributes (Failure)

### 取得賣場商品所有買家資訊 [GET /stream/{stream-id}/order/{product-id}/summary]

- Parameters
    - `stream-id`: `f0185907-9b22-4f6b-8593-cf0837f80b45` - 直播 ID

- Response 200 (application/json)
    - Attributes (Success)
        - `data` (array, fixed)
            - (object)
                - `id`: `ccfcbb7c-63d0-4d0f-9020-c19e307e97dc`
                - `recipient_name`: `王大名`
                - `recipient_address`: `台南市東區仁和路八段9號`
                - `recipient_phone`: `0987878787`

### 取得賣場所有買家資訊 [GET /stream/{stream-id}/order/summary]

- Parameters
    - `stream-id`: `f0185907-9b22-4f6b-8593-cf0837f80b45` - 直播 ID

- Response 200 (application/json)
    - Attributes (Success)
        - `data` (array, fixed)
            - (object)
                - `id`: `ccfcbb7c-63d0-4d0f-9020-c19e307e97dc`
                - `recipient_name`: `王大名`
                - `recipient_address`: `台南市東區仁和路八段9號`
                - `recipient_phone`: `0987878787`

### 取得賣場某買家所有購買 [GET /stream/{stream-id}/order/by-user/{user-id}/summary]

- Parameters
    - `stream-id`: `f0185907-9b22-4f6b-8593-cf0837f80b45` - 直播 ID
    - `user-id`: `a0185907-9b22-4f6b-8593-cf0837f80b45` - 使用者 ID

- Response 200 (application/json)
    - Attributes (Success)
        - `data`
            - `id`: `ccfcbb7c-63d0-4d0f-9020-c19e307e97dc`
            - `recipient_name`: `王大名`
            - `recipient_address`: `台南市東區仁和路八段9號`
            - `recipient_phone`: `0987878787`
            - `item` (array, fixed)
                - (object)
                    - `quantity`: `1` (number)
                    - `product_id`: `7de7cdbb-14f0-408d-9d19-a202c0332405`

### 訂單 [/stream/{stream-id}/order]

- Parameters
    - `stream-id`: `f0185907-9b22-4f6b-8593-cf0837f80b45` - 直播 ID

#### 取得訂單列表 [GET]

若使用者擁有該賣場，返回所有訂單。
若使用者不擁有該賣場，則返回自己的訂單。

預設以創建時間升冪排序。

- Parameters
    - `stream-id`: `f0185907-9b22-4f6b-8593-cf0837f80b45` - 直播 ID

- Response 200 (application/json)
    - Attributes (Success)
        - `data` (array[Order], fixed)

### 新增訂單 [POST]

送出時的 `quantity` 是調整。例如原本已經訂了 `5`，此時送出 `1`，則結果為 `6`

- Request (application/json)
    - Attributes
        - `quantity`: `1` (number)
        - `product_id`: `7de7cdbb-14f0-408d-9d19-a202c0332405`

- Response 200 (application/json)
    - Attributes
        - `quantity`: `1` (number) - 目前已買數量

<!-- ### 取得訂單概要 [GET /stream/{stream&#45;id}/order] -->
<!--  -->
<!-- 只有賣場擁有人可以呼叫。 -->
<!--  -->
<!-- &#45; Parameters -->
<!--     &#45; `stream&#45;id`: `f0185907&#45;9b22&#45;4f6b&#45;8593&#45;cf0837f80b45` &#45; 直播 ID -->
<!--  -->
<!-- &#45; Response 200 (application/json) -->
<!--     &#45; Attributes (Success) -->
<!--         &#45; `data` (array, fixed) -->
<!--             &#45; `quantity`: `1` (number) -->
<!--             &#45; `product_id`: `7de7cdbb&#45;14f0&#45;408d&#45;9d19&#45;a202c0332405` -->
<!--  -->
# Data Structure
## Success
- `success`: `true` (boolean)

## Failure
- `success`: `false` (boolean)

## BuyerInfo
- `data_filled`: `true` (boolean)
- `data`
    - `recipient_name`: `王大名`
    - `recipient_address`: `台南市東區仁和路八段9號`
    - `recipient_phone`: `0987878787`

## Product
- `title`: `商品1`
- `price`: `100` (number)
- `created_at`: `2018-01-01T00:00:00.000Z`
- `image_url`: `https://fakeimg.pl/200x200`
- `id`: `1` (number) - 項次

## Order
- `id`: `ccfcbb7c-63d0-4d0f-9020-c19e307e97dc`
- `quantity`: `100` (number)
- `buyer` (BuyerInfo)
- `created_at`: `2018-01-01T00:00:00.000Z`
