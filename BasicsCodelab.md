# Compose入门



## 1.设置不同的背景颜色

用 `Surface` 包围 `Text` 可组合项

```
@Composable
fun Greeting(name: String, modifier: Modifier = Modifier) {
    Surface(color = MaterialTheme.colorScheme.primary) {
        Text(
            text = "Hello $name!",
            modifier = modifier
        )
    }
}
```

![8cd9258af95884b6d1b723ac9192e0f](D:\development\AndroidStudioProjects\BasicsCodelab1\8cd9258af95884b6d1b723ac9192e0f.png)

## 2.为界面上的 `Text` 添加内边距

使用 `Modifier.padding()` 创建内边距修饰符

```
import androidx.compose.foundation.layout.padding
import androidx.compose.ui.unit.dp

@Composable
fun Greeting(name: String, modifier: Modifier = Modifier) {
    Surface(color = MaterialTheme.colorScheme.primary) {
        Text(
            text = "Hello $name!",
            modifier = modifier.padding(24.dp)
        )
    }
}
```

![dc388d5f58dab97b4b71f27ca1f3772](D:\development\AndroidStudioProjects\BasicsCodelab1\dc388d5f58dab97b4b71f27ca1f3772.png)

## 3.重复使用可组合项

创建一个名为 `MyApp` 的可组合项，该组合项中包含问候语，现在可以重复使用 `MyApp` 可组合项，省去 `onCreate` 回调和预览，从而避免重复编写代码。

```
package com.example.basicscodelab

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.padding
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Surface
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import com.example.basicscodelab.ui.theme.BasicsCodelabTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            BasicsCodelabTheme {
                MyApp(modifier = Modifier.fillMaxSize())
            }
        }
    }
}

@Composable
fun MyApp(modifier: Modifier = Modifier) {
    Surface(
        modifier = modifier,
        color = MaterialTheme.colorScheme.background
    ) {
        Greeting("Android")
    }
}

@Composable
fun Greeting(name: String, modifier: Modifier = Modifier) {
    Surface(color = MaterialTheme.colorScheme.primary) {
        Text(
            text = "Hello $name!",
            modifier = modifier.padding(24.dp)
        )
    }
}

@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
    BasicsCodelabTheme {
        MyApp()
    }
}
```

## 4.创建Compose中的列和行

### 4.1 Compose 和 Kotlin

```
import androidx.compose.foundation.layout.Column

@Composable
fun Greeting(name: String, modifier: Modifier = Modifier) {
    Surface(color = MaterialTheme.colorScheme.primary) {
        Column(modifier = modifier.padding(24.dp)) {
            Text(text = "Hello ")
            Text(text = name)
        }
    }
}

```

![d247118b9675943e8f18c8336eea6a5](D:\development\AndroidStudioProjects\BasicsCodelab1\d247118b9675943e8f18c8336eea6a5.png)

```
@Composable
fun MyApp(
    modifier: Modifier = Modifier,
    names: List<String> = listOf("World", "Compose")
) {
    Column(modifier) {
        for (name in names) {
            Greeting(name = name)
        }
    }
}

```

![ee9f1307674004e9bf6fbcfad54a04f](D:\development\AndroidStudioProjects\BasicsCodelab1\ee9f1307674004e9bf6fbcfad54a04f.png)

```
@Preview(showBackground = true, widthDp = 320)
@Composable
fun GreetingPreview() {
    BasicsCodelabTheme {
        MyApp()
    }
}
```

![5cca462c45d004af6e4f0ab4ce8e18a](D:\development\AndroidStudioProjects\BasicsCodelab1\5cca462c45d004af6e4f0ab4ce8e18a.png)

```
import androidx.compose.foundation.layout.fillMaxWidth

@Composable
fun MyApp(
    modifier: Modifier = Modifier,
    names: List<String> = listOf("World", "Compose")
) {
    Column(modifier = modifier.padding(vertical = 4.dp)) {
        for (name in names) {
            Greeting(name = name)
        }
    }
}

@Composable
fun Greeting(name: String, modifier: Modifier = Modifier) {
    Surface(
        color = MaterialTheme.colorScheme.primary,
        modifier = modifier.padding(vertical = 4.dp, horizontal = 8.dp)
    ) {
        Column(modifier = Modifier.fillMaxWidth().padding(24.dp)) {
            Text(text = "Hello ")
            Text(text = name)
        }
    }
}
```

![c53b4926a129d2d3c200b9cb9d53644](D:\development\AndroidStudioProjects\BasicsCodelab1\c53b4926a129d2d3c200b9cb9d53644.png)

### 4.2 添加按钮

```
import androidx.compose.foundation.layout.Row
import androidx.compose.material3.ElevatedButton

@Composable
fun Greeting(name: String, modifier: Modifier = Modifier) {
    Surface(
        color = MaterialTheme.colorScheme.primary,
        modifier = modifier.padding(vertical = 4.dp, horizontal = 8.dp)
    ) {
        Row(modifier = Modifier.padding(24.dp)) {
            Column(modifier = Modifier.weight(1f)) {
                Text(text = "Hello ")
                Text(text = name)
            }
            ElevatedButton(
                onClick = { /* TODO */ }
            ) {
                Text("Show more")
            }
        }
    }
}

```

![2d194ad33d79dd1d0e4e6fc1a9b395a](D:\development\AndroidStudioProjects\BasicsCodelab1\2d194ad33d79dd1d0e4e6fc1a9b395a.png)

## 5.Compose中的状态

```
@Composable
fun Greeting(name: String, modifier: Modifier = Modifier) {
    val expanded = remember { mutableStateOf(false) }
    val extraPadding = if (expanded.value) 48.dp else 0.dp
    Surface(
        color = MaterialTheme.colorScheme.primary,
        modifier = modifier.padding(vertical = 4.dp, horizontal = 8.dp)
    ) {
        Row(modifier = Modifier.padding(24.dp)) {
            Column(
                modifier = Modifier
                    .weight(1f)
                    .padding(bottom = extraPadding)
            ) {
                Text(text = "Hello ")
                Text(text = name)
            }
            ElevatedButton(
                onClick = { expanded.value = !expanded.value }
            ) {
                Text(if (expanded.value) "Show less" else "Show more")
            }
        }
    }
}
```

![image-20250423103541113](D:\development\AndroidStudioProjects\BasicsCodelab1\image-20250423103541113.png)

![image-20250423103612297](D:\development\AndroidStudioProjects\BasicsCodelab1\image-20250423103612297.png)