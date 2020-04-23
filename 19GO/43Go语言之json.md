Go语言对json的解析函数在encoding/json包里面，主要是编码和解码两个函数。

# Marshal函数

    
    
    func Marshal(v interface{}) ([]byte, error)

Marshal函数返回v的json编码

**注意** ：

布尔类型编码为json布尔类型。

浮点数、整数和Number类型的值编码为json数字类型。

字符串编码为json字符串。

数组和切片类型的值编码为json数组，但[]byte编码为base64编码字符串，nil切片编码为null

结构体的值编码为json对象。每一个导出字段变成该对象的一个成员，除非以下两种情况：

    
    
    字段的标签是"-"
    字段是空值，而其标签指定了omitempty选项

空值是false、0、""、nil指针、nil接口、长度为0的数组、切片、映射。对象默认键字符串是结构体的字段名，但可以在结构体字段的标签里指定。结构体标签值里的"json"键为键名，后跟可选的逗号和选项，举例如下：

    
    
    Age int `json:"-"` // 字段被本包忽略
    Name string `json:"myName"` // 字段在json里的键为"myName"
    Sex int `json:"myName,omitempty"` // 字段在json里的键为"myName"且如果字段为空值将在对象中省略掉
    Hobby int `json:",omitempty"`// 字段在json里的键为"Hobby"（默认值），但如果字段为空值会跳过；注意前导的逗号

"string"选项标记一个字段在编码json时应编码为字符串。它只适用于字符串、浮点数、整数类型的字段

    
    
    Int64String int64 `json:",string"`

如果键名是只含有unicode字符、数字、美元符号、百分号、连字符、下划线和斜杠的非空字符串，将使用它代替字段名。

匿名的结构体字段一般序列化为他们内部的导出字段就好像位于外层结构体中一样。如果一个匿名结构体字段的标签给其提供了键名，则会使用键名代替字段名，而不视为匿名。

Go结构体字段的可视性规则用于供json决定那个字段应该序列化或反序列化时是经过修正了的。如果同一层次有多个（匿名）字段且该层次是最小嵌套的（嵌套层次则使用默认go规则），会应用如下额外规则：

1）json标签为"-"的匿名字段强行忽略，不作考虑；

2）json标签提供了键名的匿名字段，视为非匿名字段；

3）其余字段中如果只有一个匿名字段，则使用该字段；

4）其余字段中如果有多个匿名字段，但压平后不会出现冲突，所有匿名字段压平；

5）其余字段中如果有多个匿名字段，但压平后出现冲突，全部忽略，不产生错误。

对匿名结构体字段的管理是从go1.1开始的，在之前的版本，匿名字段会直接忽略掉。

Map类型的值编码为json对象。Map的键必须是字符串，对象的键直接使用映射的键。

指针类型的值编码为其指向的值（的json编码）。nil指针编码为null。

接口类型的值编码为接口内保持的具体类型的值（的json编码）。nil接口编码为null。

**通道、复数、函数类型的值不能编码进json。会导致Marshal函数返回UnsupportedTypeError错误**

# Unmarshal函数

    
    
    func Unmarshal(data []byte, v interface{}) error

Unmarshal函数解析json编码的数据并将结果存入v指向的值。

Unmarshal和Marshal做相反的操作，必要时申请map、切片或指针，遵循如下规则：

要将json数据解码写入一个指针，Unmarshal函数首先处理json数据是json字面值null的情况。此时，函数将指针设为nil；否则，函数将json数据解码写入指针指向的值；如果指针本身是nil，函数会先申请一个值并使指针指向它。

要将json数据解码写入一个结构体，函数会匹配输入对象的键和Marshal使用的键（结构体字段名或者它的标签指定的键名），优先选择精确的匹配，但也接受大小写不敏感的匹配。

要将json数据解码写入一个接口类型值，函数会将数据解码为如下类型写入接口：

    
    
    Bool                   对应JSON布尔类型
    float64                对应JSON数字类型
    string                 对应JSON字符串类型
    []interface{}          对应JSON数组
    map[string]interface{} 对应JSON对象
    nil                    对应JSON的null

如果一个JSON值不匹配给出的目标类型，或者如果一个json数字写入目标类型时溢出，Unmarshal函数会跳过该字段并尽量完成其余的解码操作。如果没有出现更加严重的错误，本函数会返回一个描述第一个此类错误的详细信息的UnmarshalTypeError。

JSON的null值解码为go的接口、指针、切片时会将它们设为nil，因为null在json里一般表示“不存在”。解码json的null值到其他go类型时，不会造成任何改变，也不会产生错误。

当解码字符串时，不合法的utf-8或utf-16代理（字符）对不视为错误，而是将非法字符替换为unicode字符U+FFFD。

# 示例

### Golang - 序列化结构体

    
    
    package main
    
    import (
        "encoding/json"
        "fmt"
    )
    
    //定义一个简单的结构体 Person
    type Person struct {
        Name     string
        Age      int
        Birthday string
        Sex      float32
        Hobby    string
    }
    
    //写一个 testStruct()结构体的序列化方法
    func testStruct() {
        person := Person{
            Name:     "小崽子",
            Age:      50,
            Birthday: "2019-09-27",
            Sex:      1000.01,
            Hobby:    "泡妞",
        }
    
        // 将Monster结构体序列化
        data, err := json.Marshal(&person)
        if err != nil {
            fmt.Printf("序列化错误 err is %v", err)
        }
        //输出序列化结果
        fmt.Printf("person序列化后 = %v", string(data))
        //反序列化
        person2 := Person{}
        json.Unmarshal(data,&person2)
        fmt.Println(person2)
    }
    func main()  {
        testStruct()
    
    }

### Golang - 序列化map

    
    
    package main
    
    import (
        "encoding/json"
        "fmt"
    )
    
    func testMap() {
        //定义一个map
        var a map[string]interface{}
        //使用map之前 必须make一下
        a = make(map[string]interface{})
        a["name"] = "小崽子"
        a["age"] = 8
        a["address"] = "上海市浦东新区"
    
        // 将a map结构体序列化
        data, err := json.Marshal(a)
        if err != nil {
            fmt.Printf("序列化错误 err is %v", err)
        }
        //输出序列化结果
        fmt.Printf("map序列化后 = %v", string(data))
        //反序列化
        var a1 map[string]interface{}
        json.Unmarshal(data,&a1)
        fmt.Println(a1)
    }
    func main() {
        testMap()
    
    }

Golang - 序列化slice

    
    
    package main
    
    import (
        "encoding/json"
        "fmt"
    )
    
    // slice进行序列化
    func testSlice() {
        var slice []map[string]interface{} // 定义了一个切片，里面是map格式 map[string]interface{}
        var m1 map[string]interface{}      //定义切片中的第一个map M1
        m1 = make(map[string]interface{})
        m1["name"] = "小崽子"
        m1["age"] = 16
        m1["address"] = [2]string{"上海市", "浦东新区"}
        slice = append(slice, m1)
    
        var m2 map[string]interface{} //定义切片中的第2个map M2
        m2 = make(map[string]interface{})
        m2["name"] = "大崽子"
        m2["age"] = 36
        m2["address"] = "北京市"
        slice = append(slice, m2)
    
        // 将slice进行序列化
        data, err := json.Marshal(slice)
        if err != nil {
            fmt.Printf("序列化错误 err is %v", err)
        }
        //输出序列化结果
        fmt.Printf("slice序列化后 = %v", string(data))
        //反序列化结果
        var slice2 []map[string]interface{}
        json.Unmarshal(data,&slice2)
        fmt.Println(slice2)
    }
    func main() {
        testSlice()
    
    }
    

