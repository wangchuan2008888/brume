|Moudle Name   |         description         |
|------------- |:---------------------------:|
|pickle        | output python object as file|
|glob          |glob - Filename globbing utility|
|shelve        |save data using key-value as file|
|tkinter       |simple  interface of  python standard lib|


## Python sys module 
sys模块大概是使用最为频繁的一个模块，是python系统相关工具集的核心。
### sys.path是控制python加载包时搜索路径的，默认为PYTHONPATH进行初始化
* sys.path属性是当前python环境中的搜索路径
* sys.path.append("/home/andrew/code/python")
* sys.exc_info()函数可以返回最近的异常状态，一些python的扩展可以通过检查该函数的返回值实现异常追踪，该异常需要在try-except语句中捕获
* sys.argv属性是当前运行参数的list
* sys.stdin sys.stdout sys.stderr分别是系统的标准输入流、标准输出流、标准错误流
* sys.exit()可以强制系统退出
