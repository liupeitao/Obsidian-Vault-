# 结果处理JsonResult

```java
/**
 *
 * @Title: JSONResult.java
 * @Package com.weiz.utils
 * @Description: 自定义响应数据结构
 *                      200：表示成功
 *                      500：表示错误，错误信息在msg字段中
 *                      501：bean验证错误，无论多少个错误都以map形式返回
 *                      502：拦截器拦截到用户token出错
 *                      555：异常抛出信息
 * Copyright: Copyright (c) 2016
 *
 * @author weiz
 * @date 2016年4月22日 下午8:33:36
 * @version V1.0
 */
public class JSONResult {
    // 定义jackson对象
    private static final ObjectMapper MAPPER = new ObjectMapper();
    // 响应业务状态
    private Integer code;
    // 响应消息
    private String msg;
    // 响应中的数据
    private Object data;

    public static JSONResult build(Integer status, String msg, Object data) {
        return new JSONResult(status, msg, data);
    }

    public static JSONResult ok(Object data) {
        return new JSONResult(data);
    }

    public static JSONResult ok() {
        return new JSONResult(null);
    }

    public static JSONResult errorMsg(String msg) {
        return new JSONResult(500, msg, null);
    }

    public static JSONResult errorMap(Object data) {
        return new JSONResult(501, "error", data);
    }

    public static JSONResult errorTokenMsg(String msg) {
        return new JSONResult(502, msg, null);
    }

    public static JSONResult errorException(String msg) {
        return new JSONResult(555, msg, null);
    }

    public JSONResult() {

    }

    public JSONResult(Integer status, String msg, Object data) {
        this.status = status;
        this.msg = msg;
        this.data = data;
    }

    public JSONResult(Object data) {
        this.status = 200;
        this.msg = "OK";
        this.data = data;
    }

    public Boolean isOK() {
        return this.status == 200;
    }

    /**
     *
     * @Description: 将json结果集转化为JSONResult对象
     *                          需要转换的对象是一个类
     * @param jsonData
     * @param clazz
     * @return
     *
     * @author weiz
     * @date 2016年4月22日 下午8:34:58
     */
    public static JSONResult formatToPojo(String jsonData, Class<?> clazz) {
        try {
            if (clazz == null) {
                return MAPPER.readValue(jsonData, JSONResult.class);
            }
            JsonNode jsonNode = MAPPER.readTree(jsonData);
            JsonNode data = jsonNode.get("data");
            Object obj = null;
            if (clazz != null) {
                if (data.isObject()) {
                    obj = MAPPER.readValue(data.traverse(), clazz);
                } else if (data.isTextual()) {
                    obj = MAPPER.readValue(data.asText(), clazz);
                }
            }
            return build(jsonNode.get("status").intValue(), jsonNode.get("msg").asText(), obj);
        } catch (Exception e) {
            return null;
        }
    }

    /**
     *
     * @Description: 没有object对象的转化
     * @param json
     * @return
     *
     * @author weiz
     * @date 2016年4月22日 下午8:35:21
     */
    public static JSONResult format(String json) {
        try {
            return MAPPER.readValue(json, JSONResult.class);
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null;
    }

    /**
     *
     * @Description: Object是集合转化
     *                          需要转换的对象是一个list
     * @param jsonData
     * @param clazz
     * @return
     *
     * @author weiz
     * @date 2016年4月22日 下午8:35:31
     */
    public static JSONResult formatToList(String jsonData, Class<?> clazz) {
        try {
            JsonNode jsonNode = MAPPER.readTree(jsonData);
            JsonNode data = jsonNode.get("data");
            Object obj = null;
            if (data.isArray() && data.size() > 0) {
                obj = MAPPER.readValue(data.traverse(),
                        MAPPER.getTypeFactory().constructCollectionType
(List.class, clazz));
            }
            return build(jsonNode.get("status").intValue(), jsonNode.get("msg").asText(), obj);
        } catch (Exception e) {
            return null;
        }
    }

        public String getOk() {
                return ok;
        }

        public void setOk(String ok) {
                this.ok = ok;
        }

}
```

‍

## 调用测试

```java
@RequestMapping("/getUser")
public JSONResult getUserJson(){
    User u = new User();
    u.setName("weiz222");
    u.setAge(20);
    u.setBirthday(new Date());
    u.setPassword("weiz222");
    return JSONResult.ok(u);
}
```

```json
{
        "code": 200,
        "msg": "OK",
        "data": {
                "name": "weiz222",
                "age": 20,
                "birthday": "2020-12-21 06:57:13"
        }
}
```
