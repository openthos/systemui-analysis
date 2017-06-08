
# StartupMenu移植到android 7.1

### 说明
  StartupMenu 之前是属于系统app，这次的移植是单纯将其认为是一个普通的app，由于在openthos 5.0上对底层有很多改动，所以StartupMenu上有关于底层改动较多的地方也进行了调整
  
### 移植
  - 添加编译
        在~/ljh/multiwindow/build/target/product/core.mk文件中加入StartupMenu这个应用包名，这样编译的时候就会编译StartupMenu了
    
  - 整体移植
        将之前在openthos 5.0的StartupMenu的项目工程整体添加到android 7.1的 ~/ljh/multiwindow/packages/apps 目录下
        
  - 编译检查
        然后开始编译镜像，这个时候会报一些错误，比如缺失哪些属性，某些方法未定义什么的，然后回归openthos 5。0的源码，找到这些属性或者方法加到android 7.0上，直到编译通过
        
   - 测试运行
         通过编译后就开始装机测试，然后再次进行bug修改，毕竟android 7.0和5.0还是有一定差别的
