# iOS 개발자가 공부하는 Kotlin

https://kotlinlang.org/docs/typecasts.html 보고 공부중  
다른점 정리중

- Type  
Bool이 아닌 Boolean
    ```Kotlin
    val myTrue: Boolean = true
    val myFalse: Boolean = false
    val boolNull: Boolean? = null
    ```

- Array   
    ```Kotlin
    // Creates an array with values [1, 2, 3]
    val simpleArray = arrayOf(1, 2, 3)
    ```


- 변수 상수  
var은 동일한데 let이 아니라 val 임
    ```Kotlin
        var 변수
        val 상수
    ```


- When 구문
    ```Kotlin
        when(변수 or 상수) {
            값1 -> {
                // Code ...
            } 
            값2 -> {
                // Code ...
            } 
            else {
                // Code ...
            }
        }
    ```

- 메소드  
func이 아니라 fun
리턴값은 ->이 아닌 :
    ```Kotlin
        fun sum(a: Int, b: Int): Int {
            return a + b
        }    
    ```


- class  
참고로 class는 기본이 final임. (Swift에서는 기본이 open인데)  
그래서 final을 없애고 싶다면 open class 형식으로 사용해야됨.  


