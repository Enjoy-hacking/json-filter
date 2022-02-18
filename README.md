# json-filter
golang的json过滤器，随意选择字段，随意输出指定结构体的字段，目前还不能使用，只实现了部分功能

```go

type User struct {
	Name string `json:"name,select(justName|foo)"`
	Age  int    `json:",select(res|article)"`
	//不自定义json字段名使用结构体字段名的话，tag首字符必须是","

	LongName string `json:"long_name,select(foo)"`
	Hobby    string `json:"hobby,select(res|foo)"`
	Books    []Book `json:"books,select()"`
	Book     *Book  `json:"book,select(res|foo)"`
}

type Book struct {
	Page  int    `json:"page,select(req|foo)"`
	Price string `json:"price,select(res|foo)"`
	Title string `json:"title"`
}

model := User{
		Name:  "boyan",
		Age:   20,
		Hobby: "coding",
		Books: []Book{
			{Page: 10, Price: "199.9"},
			{Page: 100, Price: "1999.9"},
		},
		LongName: "long name",
		Book: &Book{
			Price: "18.8",
			Page:  19,
			Title: "c++从研发到脱发",
		},
	}
	
	
	fmt.Println(SelectMarshal("res", &model))                             
	//---->>output 输出以下结果：                                               
       {                                                                 
            "Age":20,                                                    
            "book":{                                                      
                "price":"18.8"                                           
			},                                                           
            "hobby":"coding"                                             
        }                                                                
                                                                          
	fmt.Println(SelectMarshal("justName", model))                         
	//---->>output 输出以下结果：                                                
    {"name":"boyan"}                                                     
                                                                         
	fmt.Println(SelectMarshal("foo", model))                             
	//---->>output 输出以下结果：
    {
        "book":{
            "page":19,
            "price":"18.8"
        },
        "hobby":"coding",
        "long_name":"long name",
        "name":"boyan"
    }
	
	
```

