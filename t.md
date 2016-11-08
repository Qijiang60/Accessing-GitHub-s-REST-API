现在互联网上的api真是如雨后春笋一般，几乎所有平台方都或多或少提供web api了，因为web已经把异构的系统，多样的媒介无差异的连接在了一起。github的api应该是目前用着最舒心的api了，因为其自描述的接口完全无需解释，灵活的请求方法和参数毫无门槛。
1. github的api是神马样子,看看以下三个链接的结果就完全明白了
a) https://api.github.com/
b) https://api.github.com/user和https://api.github.com/users/{github-name}
c) http://developer.github.com/v3
2. 带来的启发
1). api列表(api.xxx.com)
很多时候查阅本平台提供的api是件比较麻烦的事，直接在api的空接口上以开发友好的方式返回所有api的概况则会显得非常直观。所以api的平台方应该有个顶层接口来返回api列表。
2). 具体api(api.xxx.com/[api-name])
接口是层次递进的，api.github.com展示所有api， https://api.github.com/[api-name]则对应具体接口，当调用出错时给予提示和文档url。网上很多接口调用出错时，不会以友好方式返回错误信息和api文档地址，这个是比较粗心的。
3). api文档中心(developer.xxx.com/[version number])
平台所有文档，且能适配api版本。版本概念在所有api中都非常重要，某些api会直接提供版本参数，但大部分api都只处理最新版本，对老版本调用方式统一返回提示信息。在文档url这个地方加上版本号，方便知道目前接口的版本。还有文档的组织实际上是给开发者看的，那么形式就必须简单试用，github这种文档格式就非常好：接口介绍 -> 请求方法 -> 参数说明 -> example(request + response)
4). api请求/响应格式适配
get参数使用标准格式，post方法使用Json。这些细微改变使用工具构建请求更加容易。同时api_url/{sp1}/{sp2}?s3=yyy等价于api_url?s1=sp1&s2=sp2&s3=yyy这种形式的适配让接口更加灵活。由于目前互联网传输中最流行的数据格式是json，response统一都用json做处理。当然也马目前很多平台api带返回格式参数的，通常提供xml和json返回。
5). api粒度
api接口的划分是个技术活，不能太细也不能太粗。逻辑上就有差别的就拆分成两条接口，逻辑上无差别，针对个人属性的，可以用类似与api_usrl/{user_name}?xxxxx的方式来扩展api进行适配。
