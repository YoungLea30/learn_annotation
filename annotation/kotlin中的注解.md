# kotlin中的注解
介绍一下 kotlin 中注解和 java 差异和以及使用要注意的地方  
<br/>

**自定一个注解，以及 KClass**  
```kotlin
@Target(AnnotationTarget.FUNCTION)
@Retention(AnnotationRetention.RUNTIME)
annotation class MessageReceiver(
    val dataClass: KClass<*>,
    val messageType: MessageType = MessageType.EVENT
)
```
**@Deprecated**  
假如我们有一个doLogin()方法，已经弃用，希望引导用户使用一个新的 api，我们可以这样处理  
```kotlin
@Deprecated(
    message = "Use simpler login instead with correct parameter order",
    replaceWith = ReplaceWith("login(usr, pwd)")
)
fun doLogin(pwd: String, usr : String) {
    login(usr, pwd)
}

fun login(username : String, password: String) {
}
```
<br/>

在调用方法的时候可以通过 `option + enter` 提示，直接切换到新的 api  

<br/>

![login](../assets/login1.gif)  

甚至方法参数变更了，要倒入新的包，也是支持的  

```kotlin
package auth

data class Credentials(
    val username: String,
    val password: String
)
```

```kotlin
@Deprecated(
    message = "Use simpler login with Credentials",
    replaceWith = ReplaceWith(
        expression = "login(Credentials(usr, pwd))",
        imports = ["auth.Credentials"]
    )
)
fun doLogin(pwd: String, usr: String) {
}

fun doLogin(credential: Credentials) {
}
```

如果我们一个类中有多处调用这个弃用的方法，我们可以通过 `option + enter` 快速修复，同时会帮我们把需要新引入的类 import 进来  
<br/>

![login](../assets/login2.gif) 