    
    关于Protocol Buffers在c++,java和Python方面的使用大家可以参考官方文档:
   [https://developers.google.com/protocol-buffers/docs/tutorials ](https://developers.google.com/protocol-buffers/docs/tutorials  "https://developers.google.com/protocol-buffers/docs/tutorials ")
	
    下面着重讲一下Protocol Buffers在golang方面的使用情况。
 

**一 Protocol Buffers 的安装**
   
 
1. 安装标准的protocolbuf
   从这里下载 protocolbuf [https://code.google.com/p/protobuf/downloads/list](https://code.google.com/p/protobuf/downloads/list "https://code.google.com/p/protobuf/downloads/list") 这里我们选择 protobuf-2.5.0.tar.bz2   
      经过解压安装后会安装到/usr/local/bin/protoc目录下

      cd protobuf-2.5.0
     ./configure 
      make  
      make check 
      make install

2.   安装针对go环境到goprotobuf
      
      go get code.google.com/p/goprotobuf/{proto,protoc-gen-go}
    
安装成功后，可以在 `$GO_PATH/bin`下找到 `protoc-gen-go` 这个程序，那么就可以实
用`protoc-gen-go` 进行go语言的proto文件的自动生成了。



 
**二 编辑和编译源文件**
   
  编辑源文件  test.proto  
     
     package example;

      message Test {
      required string label = 1;
      optional int32 type = 2 [default=77];
      repeated int64 reps = 3;
    }

 编译源文件

    $ protoc  -I=/home/XXXX/applications/protocobuffer 
        -- go_out=/home/XXXX/applications/protocobuffer/go 
		--cpp_out=/home/XXXX/applications/protocobuffer/c++
	    /home/XXXX/applications/protocobuffer/test.proto

   

**-I  表示.proto文件所在的文件夹**   
**`go_out`  表示要生成golang语言的代码并存放到指的文件夹中**   
**`cpp_out` 表示要生成C语言的代码并存放到指定的文件夹中**  
**最后一个参数到文件路径表示要编译到消息定义文件存放路径**

生成后 `test.pb.go `文件，生成到代码为：
 

	// Code generated by protoc-gen-go.
	// source: test.proto
	// DO NOT EDIT!

	package example

	import proto "code.google.com/p/goprotobuf/proto"
	import json "encoding/json"
	import math "math"

	// Reference proto, json, and math imports to suppress error if they are not otherwise used.
	var _ = proto.Marshal
	var _ = &json.SyntaxError{}
	var _ = math.Inf
	
	type Test struct {
	Label *string `protobuf:"bytes,1,req,name=label" json:"label,omitempty"`
	Type *int32 `protobuf:"varint,2,opt,name=type,def=77" json:"type,omitempty"`
	Reps []int64 `protobuf:"varint,3,rep,name=reps" json:"reps,omitempty"`
	XXX_unrecognized []byte `json:"-"`
	}
	
	func (m *Test) Reset() { *m = Test{} }
	func (m *Test) String() string { return proto.CompactTextString(m) }
	func (*Test) ProtoMessage() {}
	
	const Default_Test_Type int32 = 77
	
	func (m *Test) GetLabel() string {
	if m != nil && m.Label != nil {
	return *m.Label
	}
	return ""
	}
	
	func (m *Test) GetType() int32 {
	if m != nil && m.Type != nil {
	return *m.Type
	}
	return Default_Test_Type
	}
	
	func (m *Test) GetReps() []int64 {
	if m != nil {
	return m.Reps
	}
	return nil
	}
	
	func init() {
	}

 

 

**三 编码和解码**
 
Google Protocol Buffer 的使用和原理    
 参见： [http://www.ibm.com/developerworks/cn/linux/l-cn-gpb/  ](http://www.ibm.com/developerworks/cn/linux/l-cn-gpb/   "http://www.ibm.com/developerworks/cn/linux/l-cn-gpb/  ")


**四 支持RPC版本的ProtocolBuf**

安装方法和go ProtocolBuf差不多，首先安装go,然后去安装官方提供的标准protocolbuf 最后运行如下的命令：

`go get code.google.com/p/protorpc`   
`go get code.google.com/p/protorpc/protoc-gen-go`

详细安装和使用方法参加：
[**http://godoc.org/code.google.com/p/protorpc**](http://godoc.org/code.google.com/p/protorpc "http://godoc.org/code.google.com/p/protorpc")