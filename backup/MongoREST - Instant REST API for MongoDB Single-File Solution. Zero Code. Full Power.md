
 Developers! Struggling with MongoDB ops? My new Binary REST Tool lets you build a full REST API in seconds – zero code, cross-platform, faster than scripts! Supports regex queries, bulk updates, everything MongoDB. Check it out on [Gumroad](https://yudaye.gumroad.com/l/mongorest) #MongoDB #DevTools #NoCode

# MongoRest (English User Manual)

A minimal, single-file RESTful API gateway that lets you talk to MongoDB over HTTP instantly — zero coding required.

## Why MongoRest?
✅ **Zero-code**: download, config, run.  
✅ **Full-featured**: `find`, `insert`, `update`, `delete`, `aggregate`, `bulkWrite` … all ready.  
✅ **Rich query syntax**: `$regex`, `$oid`, projection, sort, collation … everything MongoDB supports.  
✅ **Bulk operations**: complex multi-doc updates in one call.  
✅ **Single executable**: no dependencies, cross-platform (Windows, Linux, macOS).  
✅ **Freedom & efficiency**: freer than cloud Data-API, faster than hand-written scripts.  
✅ **Secure**: connection string via config file or environment variable only.

<img width="1356" height="402" alt="Image" src="https://github.com/user-attachments/assets/ce6cf4e9-7e39-4d6b-a0e3-230e2201b1c6" />

## Quick Start

1. **Purchase & download**  
   Grab the platform archive from [Gumroad](https://yudaye.gumroad.com/l/mongorest) and unzip.

2. **Edit `app.conf` (same folder)**  
   ```
   db.mongo.datasource = mongodb://USER:PASS@HOST:PORT
   # logging (optional)
   log.file = on
   # port (optional, default 8081)
   httpport = 8888
   ```

3. **Run**
   - Windows: double-click `mongorest-windows.exe`  
   - Linux / macOS: `chmod +x mongorest-linux && ./mongorest-linux`

4. **Test**  
   ```
   curl "http://localhost:8081/mongo?action=find&db=sample_mflix&collection=users&limit=2"
   ```


## Base URL & Method
Gateway address ：`http://your_server_ip:port/mongo?`
Supported verbs:`get`、`post`
get url demo：`http://your_server_ip:port/mongo?action=find&db=mydb&collection=mycolleciton`
post url demo：`curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -H "Cache-Control: no-cache" -H "Postman-Token: ebad650a-e52f-656f-7c91-8cda14df5cae" -d 'action=find&collection=users&db=sample_mflix&limit=2&filter={}' "http://localhost:8081/mongo"`


## Universal Parameters


| Param       | Required | Description | Example |
|-------------|----------|-------------|---------|
| `action`    | **yes**  | Operation name | `find`, `count`, `updateMany`, `deleteMany`, `aggregate`, `bulkWrite` … |
| `db`        | **yes**  | Database name | `sample_mflix` |
| `collection`| **yes**  | Collection name | `users` |
| `filter`    | no       | MongoDB query doc (JSON string) | `{"age":{"$gte":18}}` |
| `projection`| no       | Field projection (0=hide, 1=show) | `{"_id":0,"name":1}` |
| `sort`      | no       | Sort rules (1=asc, -1=desc) | `{"name":1,"age":-1}` |
| `limit`     | no       | Max docs per call (≤10 000) | `20` |
| `index`     | no       | Page index, start at 0 | `3` |
| `collation` | no       | Collation settings | `{"locale":"zh"}` |
| `upset`     | no       | Upsert flag (any value) | `upset=1` |

---

### Action Guide

#### 1. Find

*   **Action**: `find`
*   **Demo**:
*   **simple query**: `http://localhost:8081/mongo?action=find&collection=users&db=sample_mflix&limit=2&filter={}`
```json
[
  {
    "_id": "59b99db4cfa9a34dcd7885b7",
    "email": "mark_addy@gameofthron.es",
    "name": "Robert Baratheon",
    "password": "$2b$12$yGqxLG9LZpXA2xVDhuPnSOZd.VURVkz7wgOLY3pnO0s7u2S1ZO32y"
  },
  {
    "_id": "59b99db6cfa9a34dcd7885ba",
    "email": "lena_headey@gameofthron.es",
    "name": "Cersei Lannister",
    "password": "$2b$12$FExjgr7CLhNCa.oUsB9seub8mqcHzkJCFZ8heMc8CeIKOZfeTKP8m"
  }
]
```

*   **Regex search**: `localhost:8081/mongo?action=find&collection=users&db=sample_mflix&limit=2&filter={"name":{"$regex":"R"}}` 
 ```json
[
  {
    "_id": "59b99db4cfa9a34dcd7885b7",
    "email": "mark_addy@gameofthron.es",
    "name": "Robert Baratheon",
    "password": "$2b$12$yGqxLG9LZpXA2xVDhuPnSOZd.VURVkz7wgOLY3pnO0s7u2S1ZO32y"
  },
  {
    "_id": "59b99dbacfa9a34dcd7885c2",
    "email": "richard_madden@gameofthron.es",
    "name": "Robb Stark",
    "password": "$2b$12$XPLvWQW7tjWc/PX9jMVRnO8w.lR6hv144ee8pc8nDsWIAWxfwxHzy"
  }
]
  ```
*   **By ObjectId**: `localhost:8081/mongo?action=find&collection=users&db=sample_mflix&limit=2&filter={"_id":{"$oid":"59b99dbacfa9a34dcd7885c2"}}`
```json
[
  {
    "_id": "59b99dbacfa9a34dcd7885c2",
    "email": "richard_madden@gameofthron.es",
    "name": "Robb Stark",
    "password": "$2b$12$XPLvWQW7tjWc/PX9jMVRnO8w.lR6hv144ee8pc8nDsWIAWxfwxHzy"
  }
] 
   ```
 
  
*   **Projection (only return _id & email)**: `localhost:8081/mongo?action=find&collection=users&db=sample_mflix&limit=2&filter={"_id":{"$oid":"59b99dbacfa9a34dcd7885c2"}}&projection={"_id":true,"email":true}`
*   **Projection (clear _id)**: `localhost:8081/mongo?action=find&collection=users&db=sample_mflix&limit=2&filter={"_id":{"$oid":"59b99dbacfa9a34dcd7885c2"}}&projection={"_id":false}`
*   **Sorting (name ASC, email DESC)**: `localhost:8081/mongo?action=find&collection=users&db=sample_mflix&limit=5&sort={"name":1,"email":-1}`
*  **Count only**: `localhost:8081/mongo?action=count&collection=users&db=sample_mflix`
```json
{"count":185}
  ```

#### 2. Update

*   **Action**: `updateMany、findOneAndUpdate`
*   **Parameter**: `update` (json string)
*   **Demo**:
*   **Deep update**: `?action=updateMany&db=other&collection=tudi&filter={"tudi.name":"2222"}&update={"$set":{"tudi.$.status":"1"}}`

*    **findOneAndUpdate **: `localhost:8081/mongo?action=findOneAndUpdate&collection=users&db=sample_mflix&filter={"_id":{"$oid":"59b99dbacfa9a34dcd7885c2"}}&update={"$set":{"name":"11"}}`
*  ***if update success,return updated document***: 
 ```json
{
  "_id": "59b99dbacfa9a34dcd7885c2",
  "email": "richard_madden@gameofthron.es",
  "name": "11",
  "password": "$2b$12$XPLvWQW7tjWc/PX9jMVRnO8w.lR6hv144ee8pc8nDsWIAWxfwxHzy"
}
   ```
*  ***if update failed,return count document***: 
 ```json
{"MatchedCount":0,"ModifiedCount":0,"UpsertedCount":0}
   ```
*    **upset更新 **: `localhost:8081/mongo?action=updateMany&collection=users&db=sample_mflix&update={"$set":{"name":"999"}}&filter={"name":"2342342342"}&upset=1`
*  ***UpsertedCount is one，mean upset 1 row***: 
 ```json
{
  "MatchedCount": 0,
  "ModifiedCount": 0,
  "UpsertedCount": 1
}
   ```


#### 3. Insert

*   **Action**: `insertMany`
*   **Parameter**: `insert` (JSON list string)
*   **Demo**: `localhost:8081/mongo?action=insertMany&collection=users&db=sample_mflix&insert=[{"name":"01","user":"17","pwd":"01-17"}]`
```json
{"InsertedCount":1}
```

#### 4. Delete

*   **Action**: `deleteMany`
*   **Demo**:
`localhost:8081/mongo?action=deleteMany&collection=users&db=sample_mflix&filter={"name":"01"}`
```json
{"DeletedCount":1}
```
#### 5. Aggregate

*   **Action**: `aggregate`
*   **Parameter**: `filter` 
*   **Demo**:
`?action=aggregate&db=dbname&collection=domains&filter=[{"$group": {"_id": "$title", "count": {"$sum": 1}}}]`

#### 6. Bulk Write - **High level function**

*   **Action**: `bulkWrite`
*   **Parameter**: `updates` (use split `>-<` and `<->`)
*   **Syntax**: `{filter1}>-<{update1}<->{filter2}>-<{update2}`
*   **Demo**:
`#localhost:8081/mongo?action=bulkWrite&db=sample_mflix&collection=users&updates={"name":"Ned%20Stark"}>-<{"$set":{"password":"ggg111"}}<->{"name":"Robert%20Baratheon"}>-<{"$set":{"password":"ggg222"}}`
```json
{
  "MatchedCount": 2,
  "ModifiedCount": 2,
  "UpsertedCount": 0,
  "UpsertedIDs": {

  },
  "code": 200,
  "msg": "success"
}
```


## Security & Configuration
- **NEVER expose the service to the public internet** — use inside trusted networks only.  
- Store connection string in `app.conf` (same folder):
* datasource**:
`db.mongo.datasource=mongodb://{your_mongodb_user}:{password}@{ip_or_domain}:{port}`
*   **LocalHost Demo**:
`db.mongo.datasource=mongodb://yuhanger:867983@localhost:27017`
*   **RemoteIPDemo**:
`db.mongo.datasource=mongodb://yuhanger:867983@10.10.10.10:27017`
*   **MongoDB AtlasDemo**:
`db.mongo.datasource=db.mongo.datasource=mongodb+srv://user:password@cluster0.jmaxghd.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0`


## Logging
- Logs are written to `logs/` folder when `log.file=on` is set in `app.conf`.   
```
2025/09/04 20:26:49.050 [I] [util.go:136]  -------requestInfo-----------  
2025/09/04 20:26:49.050 [I] [util.go:137]  request path: /mongo  
2025/09/04 20:26:49.050 [I] [util.go:138]  client ip: [::1]  
2025/09/04 20:26:49.050 [I] [util.go:139]  action: updateMany  
2025/09/04 20:26:49.050 [I] [util.go:140]  db: sample_mflix  
2025/09/04 20:26:49.050 [I] [util.go:141]  collection: users  
2025/09/04 20:26:49.050 [I] [util.go:142]  filter: {"name":"2342342342"}  
2025/09/04 20:26:49.050 [I] [util.go:143]  update: {"$set":{"name":"999"}}  
2025/09/04 20:26:49.050 [I] [util.go:144]  updates:   
2025/09/04 20:26:49.050 [I] [util.go:145]  projection: {}  
2025/09/04 20:26:49.050 [I] [util.go:146]  limit: 10000  
2025/09/04 20:26:49.050 [I] [util.go:147]  index: 0  
2025/09/04 20:26:49.050 [I] [util.go:148]  key:   
2025/09/04 20:26:49.050 [I] [util.go:149]  sort:   
2025/09/04 20:26:49.050 [I] [util.go:150]  collation:   
2025/09/04 20:26:49.050 [I] [util.go:151]  upset: 1  
2025/09/04 20:26:49.050 [I] [util.go:152]  retdoc:    
2025/09/04 20:26:49.050 [I] [util.go:153]  bulk-upsets:   
2025/09/04 20:26:49.050 [I] [util.go:154]  -------requestInfo-----------
```

## 💰Licensing & Purchase
- Buy once on [Gumroad](https://yudaye.gumroad.com/l/mongorest) and get lifetime access.  
- **FREE tier**: read-only operations (`find` / `count`) only.  
- **FULL tier**: unlocks all actions — see comparison table on Gumroad.

## Contact
```
GMail: yuhang19890101@gmail.com
QQ   : 6686496
X      :      https://x.com/lazyisfast
```