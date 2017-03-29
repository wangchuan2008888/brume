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
### 管理工具
* os.getpid() 返回当前进程的进程号
* os.getcwd() 返回当前目录

### 系统可移植常量
* os.pathsep 系统PATH中各path的分隔符
* os.sep  系统路径的分隔符
* os.pardir 父目录
* os.curdir 当前目录
* os.linesep 系统相关的文件分隔符
### os.path  工具
* os.path.isdir()
* os.path.isfile()
* os.path.exists()
* os.path.getsize()
* os.path.splite("/home/andrew") 返回父目录和最底目录/文件的tuple
* os.path.join() 返回合并以后的路径
* os.path.abspath()  返回路径的全路径名
* os.open() 返回的文件描述符，这点与内建的open不同
* os.environ 返回系统的环境变量，python启动时初始化完成，不会动态更新
* os.getenv()获取环境变量，从os.environ中获取
* os.putenv()更新环境到shell环境，但是在os.environ 中不会获取到
