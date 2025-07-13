# 🚀 LỘ TRÌNH JETPACK COMPOSE + ANDROID
## Từ Cơ Bản Đến Chuyên Gia (16 Tuần)

---

## 📋 **TỔNG QUAN**

| 📊 Thông Tin | 📝 Chi Tiết |
|--------------|-------------|
| **Thời gian** | 16 tuần (4 tháng) |
| **Cường độ** | 15-20h/tuần |
| **Mục tiêu** | Thành thạo Jetpack Compose & Modern Android |
| **Đầu ra** | Có thể làm việc với Compose trong production |

---

## 🎯 **CẤU TRÚC LỘ TRÌNH**

### **PHASE 1: FOUNDATION (Tuần 1-4)**
- Kotlin cơ bản cho Android
- Compose fundamentals
- State management
- Basic navigation

### **PHASE 2: INTERMEDIATE (Tuần 5-8)**
- Advanced Compose
- Material Design 3
- Animations
- Side effects

### **PHASE 3: ADVANCED (Tuần 9-12)**
- Architecture patterns
- Performance optimization
- Testing
- Custom components

### **PHASE 4: EXPERT (Tuần 13-16)**
- Production-ready apps
- Advanced topics
- Real-world projects
- Deployment

---

# 📚 **PHASE 1: FOUNDATION (Tuần 1-4)**

## 🔥 **TUẦN 1: KOTLIN + ANDROID BASICS**

### **Ngày 1-2: Kotlin Fundamentals**
```kotlin
// 1. Kotlin Basics
fun main() {
    val name = "Android Developer"
    var age = 25
    
    // Null safety
    var nullableName: String? = null
    
    // Functions
    fun greet(name: String): String {
        return "Hello $name"
    }
    
    // Lambda expressions
    val numbers = listOf(1, 2, 3, 4, 5)
    val doubled = numbers.map { it * 2 }
    
    // Classes and data classes
    data class User(val name: String, val age: Int)
    
    // Extension functions
    fun String.isEmailValid(): Boolean {
        return this.contains("@")
    }
}

// 2. Coroutines basics
suspend fun fetchUserData(): User {
    delay(1000) // Simulate network call
    return User("John Doe", 30)
}

// 3. Flow basics
fun userFlow(): Flow<User> = flow {
    emit(User("User 1", 25))
    delay(1000)
    emit(User("User 2", 30))
}
```

**Thực hành:**
- Hoàn thành Kotlin Koans
- Tạo 5 data classes với different properties
- Viết 10 extension functions
- Practice coroutines với delay và async

### **Ngày 3-4: Android Basics**
```kotlin
// 1. Activity và Lifecycle
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        Log.d("MainActivity", "onCreate called")
    }
    
    override fun onStart() {
        super.onStart()
        Log.d("MainActivity", "onStart called")
    }
    
    override fun onResume() {
        super.onResume()
        Log.d("MainActivity", "onResume called")
    }
}

// 2. Context và Resources
class Utils {
    fun getStringFromResources(context: Context, resId: Int): String {
        return context.getString(resId)
    }
    
    fun dpToPx(context: Context, dp: Float): Float {
        return TypedValue.applyDimension(
            TypedValue.COMPLEX_UNIT_DIP,
            dp,
            context.resources.displayMetrics
        )
    }
}
```

**Thực hành:**
- Tạo empty Activity và log lifecycle
- Setup resources (strings, colors, dimensions)
- Hiểu về Context và các loại Context

### **Ngày 5-7: First Compose App**
```kotlin
// 1. Setup Compose
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MyAppTheme {
                GreetingScreen()
            }
        }
    }
}

// 2. First Composable
@Composable
fun GreetingScreen() {
    var name by remember { mutableStateOf("") }
    
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        TextField(
            value = name,
            onValueChange = { name = it },
            label = { Text("Enter your name") }
        )
        
        Spacer(modifier = Modifier.height(16.dp))
        
        if (name.isNotEmpty()) {
            Text(
                text = "Hello $name!",
                style = MaterialTheme.typography.headlineMedium
            )
        }
    }
}

// 3. Preview
@Preview(showBackground = true)
@Composable
fun GreetingScreenPreview() {
    MyAppTheme {
        GreetingScreen()
    }
}
```

**Dự án tuần 1: Personal Card App**
- Input fields cho name, email, phone
- Display information in a card
- Add profile image placeholder
- Basic validation

---

## 🎨 **TUẦN 2: COMPOSE FUNDAMENTALS**

### **Ngày 1-2: Basic Composables**
```kotlin
// 1. Text Composables
@Composable
fun TextExamples() {
    Column {
        // Basic text
        Text("Simple text")
        
        // Styled text
        Text(
            text = "Styled text",
            color = Color.Blue,
            fontSize = 18.sp,
            fontWeight = FontWeight.Bold,
            fontStyle = FontStyle.Italic
        )
        
        // Annotated string
        val annotatedString = buildAnnotatedString {
            withStyle(style = SpanStyle(color = Color.Red)) {
                append("Red")
            }
            append(" and ")
            withStyle(style = SpanStyle(fontWeight = FontWeight.Bold)) {
                append("Bold")
            }
        }
        Text(annotatedString)
    }
}

// 2. Button Composables
@Composable
fun ButtonExamples() {
    Column(
        verticalArrangement = Arrangement.spacedBy(8.dp)
    ) {
        // Regular button
        Button(onClick = { /* Handle click */ }) {
            Text("Regular Button")
        }
        
        // Outlined button
        OutlinedButton(onClick = { /* Handle click */ }) {
            Text("Outlined Button")
        }
        
        // Text button
        TextButton(onClick = { /* Handle click */ }) {
            Text("Text Button")
        }
        
        // Floating Action Button
        FloatingActionButton(
            onClick = { /* Handle click */ }
        ) {
            Icon(Icons.Default.Add, contentDescription = "Add")
        }
    }
}

// 3. Image Composables
@Composable
fun ImageExamples() {
    Column {
        // Vector drawable
        Image(
            painter = painterResource(id = R.drawable.ic_android),
            contentDescription = "Android icon",
            modifier = Modifier.size(100.dp)
        )
        
        // Network image with Coil
        AsyncImage(
            model = "https://example.com/image.jpg",
            contentDescription = "Network image",
            modifier = Modifier.size(100.dp),
            contentScale = ContentScale.Crop
        )
    }
}
```

### **Ngày 3-4: Layout Composables**
```kotlin
// 1. Column và Row
@Composable
fun LayoutExamples() {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.SpaceEvenly
    ) {
        // Row example
        Row(
            horizontalArrangement = Arrangement.spacedBy(16.dp),
            verticalAlignment = Alignment.CenterVertically
        ) {
            Icon(Icons.Default.Person, contentDescription = null)
            Text("User Profile")
            Icon(Icons.Default.Settings, contentDescription = null)
        }
        
        // Nested layouts
        Card(
            modifier = Modifier
                .fillMaxWidth()
                .padding(16.dp),
            elevation = CardDefaults.cardElevation(defaultElevation = 4.dp)
        ) {
            Column(
                modifier = Modifier.padding(16.dp)
            ) {
                Text(
                    text = "Card Title",
                    style = MaterialTheme.typography.headlineSmall
                )
                Spacer(modifier = Modifier.height(8.dp))
                Text(
                    text = "Card content goes here...",
                    style = MaterialTheme.typography.bodyMedium
                )
            }
        }
    }
}

// 2. Box Layout
@Composable
fun BoxExample() {
    Box(
        modifier = Modifier
            .size(200.dp)
            .background(Color.LightGray),
        contentAlignment = Alignment.Center
    ) {
        // Background
        Box(
            modifier = Modifier
                .fillMaxSize()
                .background(Color.Blue.copy(alpha = 0.3f))
        )
        
        // Center content
        Text(
            text = "Centered Text",
            color = Color.White,
            fontWeight = FontWeight.Bold
        )
        
        // Top end badge
        Box(
            modifier = Modifier
                .align(Alignment.TopEnd)
                .background(Color.Red, CircleShape)
                .size(24.dp),
            contentAlignment = Alignment.Center
        ) {
            Text("5", color = Color.White, fontSize = 12.sp)
        }
    }
}
```

### **Ngày 5-7: Modifiers Deep Dive**
```kotlin
// 1. Size và Padding
@Composable
fun ModifierExamples() {
    Column {
        // Size modifiers
        Text(
            text = "Fixed size",
            modifier = Modifier
                .size(200.dp, 50.dp)
                .background(Color.Yellow)
        )
        
        Text(
            text = "Fill max width",
            modifier = Modifier
                .fillMaxWidth()
                .height(50.dp)
                .background(Color.Green)
        )
        
        // Padding và margin
        Text(
            text = "Padded text",
            modifier = Modifier
                .padding(16.dp) // External padding
                .background(Color.Blue)
                .padding(8.dp) // Internal padding
        )
        
        // Weight in Row/Column
        Row(modifier = Modifier.fillMaxWidth()) {
            Text(
                text = "Left",
                modifier = Modifier
                    .weight(1f)
                    .background(Color.Red)
            )
            Text(
                text = "Right",
                modifier = Modifier
                    .weight(2f)
                    .background(Color.Blue)
            )
        }
    }
}

// 2. Click và Interaction
@Composable
fun InteractionModifiers() {
    var clicked by remember { mutableStateOf(false) }
    
    Column {
        // Clickable
        Text(
            text = "Click me!",
            modifier = Modifier
                .clickable { clicked = !clicked }
                .background(if (clicked) Color.Green else Color.Gray)
                .padding(16.dp)
        )
        
        // Selectable
        val options = listOf("Option 1", "Option 2", "Option 3")
        var selectedIndex by remember { mutableStateOf(0) }
        
        options.forEachIndexed { index, option ->
            Text(
                text = option,
                modifier = Modifier
                    .selectable(
                        selected = selectedIndex == index,
                        onClick = { selectedIndex = index }
                    )
                    .background(
                        if (selectedIndex == index) Color.Blue else Color.Transparent
                    )
                    .padding(16.dp)
            )
        }
    }
}
```

**Dự án tuần 2: Recipe App UI**
- Grid layout với recipe cards
- Different text styles
- Images với placeholders
- Interactive buttons

---

## 🔄 **TUẦN 3: STATE MANAGEMENT**

### **Ngày 1-2: Remember và MutableState**
```kotlin
// 1. Basic State
@Composable
fun CounterApp() {
    var count by remember { mutableStateOf(0) }
    
    Column(
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center,
        modifier = Modifier.fillMaxSize()
    ) {
        Text(
            text = "Count: $count",
            style = MaterialTheme.typography.headlineLarge
        )
        
        Row {
            Button(onClick = { count-- }) {
                Text("-")
            }
            Spacer(modifier = Modifier.width(16.dp))
            Button(onClick = { count++ }) {
                Text("+")
            }
        }
    }
}

// 2. Complex State
data class TodoItem(
    val id: Int,
    val text: String,
    val completed: Boolean = false
)

@Composable
fun TodoApp() {
    var todos by remember { mutableStateOf(listOf<TodoItem>()) }
    var newTodoText by remember { mutableStateOf("") }
    
    Column(modifier = Modifier.padding(16.dp)) {
        // Add todo
        Row(
            modifier = Modifier.fillMaxWidth(),
            verticalAlignment = Alignment.CenterVertically
        ) {
            TextField(
                value = newTodoText,
                onValueChange = { newTodoText = it },
                modifier = Modifier.weight(1f),
                placeholder = { Text("Add new todo") }
            )
            Button(
                onClick = {
                    if (newTodoText.isNotBlank()) {
                        todos = todos + TodoItem(
                            id = todos.size,
                            text = newTodoText
                        )
                        newTodoText = ""
                    }
                }
            ) {
                Text("Add")
            }
        }
        
        Spacer(modifier = Modifier.height(16.dp))
        
        // Todo list
        LazyColumn {
            items(todos) { todo ->
                TodoItemRow(
                    todo = todo,
                    onToggle = { 
                        todos = todos.map {
                            if (it.id == todo.id) it.copy(completed = !it.completed)
                            else it
                        }
                    },
                    onDelete = {
                        todos = todos.filter { it.id != todo.id }
                    }
                )
            }
        }
    }
}

@Composable
fun TodoItemRow(
    todo: TodoItem,
    onToggle: () -> Unit,
    onDelete: () -> Unit
) {
    Row(
        modifier = Modifier
            .fillMaxWidth()
            .padding(vertical = 4.dp),
        verticalAlignment = Alignment.CenterVertically
    ) {
        Checkbox(
            checked = todo.completed,
            onCheckedChange = { onToggle() }
        )
        Text(
            text = todo.text,
            modifier = Modifier.weight(1f),
            textDecoration = if (todo.completed) TextDecoration.LineThrough else null
        )
        IconButton(onClick = onDelete) {
            Icon(Icons.Default.Delete, contentDescription = "Delete")
        }
    }
}
```

### **Ngày 3-4: State Hoisting**
```kotlin
// 1. State Hoisting Example
@Composable
fun ParentComponent() {
    var sharedState by remember { mutableStateOf("") }
    
    Column {
        // Pass state down và callbacks up
        InputComponent(
            value = sharedState,
            onValueChange = { sharedState = it }
        )
        
        DisplayComponent(value = sharedState)
        
        ResetComponent(onReset = { sharedState = "" })
    }
}

@Composable
fun InputComponent(
    value: String,
    onValueChange: (String) -> Unit
) {
    TextField(
        value = value,
        onValueChange = onValueChange,
        label = { Text("Enter text") }
    )
}

@Composable
fun DisplayComponent(value: String) {
    if (value.isNotEmpty()) {
        Text("You entered: $value")
    }
}

@Composable
fun ResetComponent(onReset: () -> Unit) {
    Button(onClick = onReset) {
        Text("Reset")
    }
}

// 2. Complex State Hoisting
data class FormState(
    val name: String = "",
    val email: String = "",
    val phone: String = "",
    val nameError: String? = null,
    val emailError: String? = null,
    val phoneError: String? = null
)

@Composable
fun FormScreen() {
    var formState by remember { mutableStateOf(FormState()) }
    
    fun validateAndSubmit() {
        val nameError = if (formState.name.isBlank()) "Name is required" else null
        val emailError = if (!formState.email.contains("@")) "Invalid email" else null
        val phoneError = if (formState.phone.length < 10) "Invalid phone" else null
        
        formState = formState.copy(
            nameError = nameError,
            emailError = emailError,
            phoneError = phoneError
        )
        
        if (nameError == null && emailError == null && phoneError == null) {
            // Submit form
        }
    }
    
    UserForm(
        formState = formState,
        onFormUpdate = { formState = it },
        onSubmit = ::validateAndSubmit
    )
}

@Composable
fun UserForm(
    formState: FormState,
    onFormUpdate: (FormState) -> Unit,
    onSubmit: () -> Unit
) {
    Column(modifier = Modifier.padding(16.dp)) {
        OutlinedTextField(
            value = formState.name,
            onValueChange = { onFormUpdate(formState.copy(name = it, nameError = null)) },
            label = { Text("Name") },
            isError = formState.nameError != null,
            supportingText = formState.nameError?.let { { Text(it) } }
        )
        
        OutlinedTextField(
            value = formState.email,
            onValueChange = { onFormUpdate(formState.copy(email = it, emailError = null)) },
            label = { Text("Email") },
            isError = formState.emailError != null,
            supportingText = formState.emailError?.let { { Text(it) } }
        )
        
        OutlinedTextField(
            value = formState.phone,
            onValueChange = { onFormUpdate(formState.copy(phone = it, phoneError = null)) },
            label = { Text("Phone") },
            isError = formState.phoneError != null,
            supportingText = formState.phoneError?.let { { Text(it) } }
        )
        
        Spacer(modifier = Modifier.height(16.dp))
        
        Button(
            onClick = onSubmit,
            modifier = Modifier.fillMaxWidth()
        ) {
            Text("Submit")
        }
    }
}
```

**Dự án tuần 3: Shopping Cart App**
- Add/remove items
- Quantity controls
- Calculate total
- Form validation

---

## 🧭 **TUẦN 4: NAVIGATION & LISTS**

### **Ngày 1-3: Navigation Compose**
```kotlin
// 1. Setup Navigation
@Composable
fun AppNavigation() {
    val navController = rememberNavController()
    
    NavHost(
        navController = navController,
        startDestination = "home"
    ) {
        composable("home") {
            HomeScreen(
                onNavigateToDetail = { itemId ->
                    navController.navigate("detail/$itemId")
                },
                onNavigateToProfile = {
                    navController.navigate("profile")
                }
            )
        }
        
        composable(
            "detail/{itemId}",
            arguments = listOf(navArgument("itemId") { type = NavType.StringType })
        ) { backStackEntry ->
            val itemId = backStackEntry.arguments?.getString("itemId")
            DetailScreen(
                itemId = itemId,
                onNavigateBack = { navController.popBackStack() }
            )
        }
        
        composable("profile") {
            ProfileScreen(
                onNavigateBack = { navController.popBackStack() }
            )
        }
    }
}

// 2. Screens
@Composable
fun HomeScreen(
    onNavigateToDetail: (String) -> Unit,
    onNavigateToProfile: () -> Unit
) {
    val items = remember {
        listOf("Item 1", "Item 2", "Item 3", "Item 4", "Item 5")
    }
    
    Column(modifier = Modifier.padding(16.dp)) {
        Button(
            onClick = onNavigateToProfile,
            modifier = Modifier.fillMaxWidth()
        ) {
            Text("Go to Profile")
        }
        
        Spacer(modifier = Modifier.height(16.dp))
        
        Text("Items:", style = MaterialTheme.typography.headlineMedium)
        
        Spacer(modifier = Modifier.height(8.dp))
        
        LazyColumn {
            items(items) { item ->
                Card(
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(vertical = 4.dp)
                        .clickable { onNavigateToDetail(item) },
                    elevation = CardDefaults.cardElevation(defaultElevation = 2.dp)
                ) {
                    Text(
                        text = item,
                        modifier = Modifier.padding(16.dp)
                    )
                }
            }
        }
    }
}

@Composable
fun DetailScreen(
    itemId: String?,
    onNavigateBack: () -> Unit
) {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp)
    ) {
        Row(
            verticalAlignment = Alignment.CenterVertically
        ) {
            IconButton(onClick = onNavigateBack) {
                Icon(Icons.Default.ArrowBack, contentDescription = "Back")
            }
            Text(
                text = "Detail Screen",
                style = MaterialTheme.typography.headlineMedium
            )
        }
        
        Spacer(modifier = Modifier.height(16.dp))
        
        Text(
            text = "Item ID: $itemId",
            style = MaterialTheme.typography.bodyLarge
        )
        
        Spacer(modifier = Modifier.height(16.dp))
        
        Text(
            text = "This is the detail screen for $itemId. Here you can show detailed information about the selected item.",
            style = MaterialTheme.typography.bodyMedium
        )
    }
}
```

### **Ngày 4-7: LazyColumn & Lists**
```kotlin
// 1. Basic LazyColumn
@Composable
fun BasicList() {
    val items = (1..100).map { "Item $it" }
    
    LazyColumn {
        items(items) { item ->
            Text(
                text = item,
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(16.dp)
            )
        }
    }
}

// 2. Complex List with different item types
sealed class ListItem {
    data class Header(val title: String) : ListItem()
    data class Content(val id: Int, val title: String, val description: String) : ListItem()
    data class Footer(val text: String) : ListItem()
}

@Composable
fun ComplexList() {
    val items = remember {
        buildList {
            add(ListItem.Header("Section 1"))
            repeat(3) { index ->
                add(ListItem.Content(index, "Title $index", "Description $index"))
            }
            add(ListItem.Header("Section 2"))
            repeat(5) { index ->
                add(ListItem.Content(index + 3, "Title ${index + 3}", "Description ${index + 3}"))
            }
            add(ListItem.Footer("End of list"))
        }
    }
    
    LazyColumn {
        items(items) { item ->
            when (item) {
                is ListItem.Header -> HeaderItem(item.title)
                is ListItem.Content -> ContentItem(item)
                is ListItem.Footer -> FooterItem(item.text)
            }
        }
    }
}

@Composable
fun HeaderItem(title: String) {
    Text(
        text = title,
        style = MaterialTheme.typography.headlineSmall,
        color = MaterialTheme.colorScheme.primary,
        modifier = Modifier
            .fillMaxWidth()
            .background(MaterialTheme.colorScheme.surfaceVariant)
            .padding(16.dp)
    )
}

@Composable
fun ContentItem(item: ListItem.Content) {
    Card(
        modifier = Modifier
            .fillMaxWidth()
            .padding(horizontal = 16.dp, vertical = 4.dp),
        elevation = CardDefaults.cardElevation(defaultElevation = 2.dp)
    ) {
        Column(modifier = Modifier.padding(16.dp)) {
            Text(
                text = item.title,
                style = MaterialTheme.typography.titleMedium
            )
            Text(
                text = item.description,
                style = MaterialTheme.typography.bodyMedium,
                color = MaterialTheme.colorScheme.onSurfaceVariant
            )
        }
    }
}

@Composable
fun FooterItem(text: String) {
    Text(
        text = text,
        style = MaterialTheme.typography.bodySmall,
        color = MaterialTheme.colorScheme.onSurfaceVariant,
        modifier = Modifier
            .fillMaxWidth()
            .padding(16.dp),
        textAlign = TextAlign.Center
    )
}

// 3. LazyGrid
@Composable
fun PhotoGrid() {
    val photos = remember { (1..50).map { "Photo $it" } }
    
    LazyVerticalGrid(
        columns = GridCells.Fixed(2),
        contentPadding = PaddingValues(16.dp),
        horizontalArrangement = Arrangement.spacedBy(8.dp),
        verticalArrangement = Arrangement.spacedBy(8.dp)
    ) {
        items(photos) { photo ->
            PhotoItem(photo)
        }
    }
}

@Composable
fun PhotoItem(photo: String) {
    Card(
        modifier = Modifier
            .fillMaxWidth()
            .aspectRatio(1f),
        elevation = CardDefaults.cardElevation(defaultElevation = 4.dp)
    ) {
        Box(
            modifier = Modifier
                .fillMaxSize()
                .background(
                    Brush.linearGradient(
                        listOf(Color.Blue, Color.Purple)
                    )
                ),
            contentAlignment = Alignment.Center
        ) {
            Text(
                text = photo,
                color = Color.White,
                fontWeight = FontWeight.Bold
            )
        }
    }
}
```

**Dự án tuần 4: News App**
- Multi-screen navigation
- Article list với images
- Detail screen
- Categories
- Search functionality

---

# 🎨 **PHASE 2: INTERMEDIATE (Tuần 5-8)**

## 🎭 **TUẦN 5: MATERIAL DESIGN 3**

### **Ngày 1-2: Theming System**
```kotlin
// 1. Color Scheme
val LightColorScheme = lightColorScheme(
    primary = Color(0xFF6750A4),
    onPrimary = Color(0xFFFFFFFF),
    primaryContainer = Color(0xFFEADDFF),
    onPrimaryContainer = Color(0xFF21005D),
    secondary = Color(0xFF625B71),
    onSecondary = Color(0xFFFFFFFF),
    secondaryContainer = Color(0xFFE8DEF8),
    onSecondaryContainer = Color(0xFF1D192B),
    tertiary = Color(0xFF7D5260),
    onTertiary = Color(0xFFFFFFFF),
    tertiaryContainer = Color(0xFFFFD8E4),
    onTertiaryContainer = Color(0xFF31111D),
    error = Color(0xFFBA1A1A),
    onError = Color(0xFFFFFFFF),
    errorContainer = Color(0xFFFFDAD6),
    onErrorContainer = Color(0xFF410002),
    background = Color(0xFFFFFBFE),
    onBackground = Color(0xFF1C1B1F),
    surface = Color(0xFFFFFBFE),
    onSurface = Color(0xFF1C1B1F),
    surfaceVariant = Color(0xFFE7E0EC),
    onSurfaceVariant = Color(0xFF49454F),
    outline = Color(0xFF79747E),
    inverseOnSurface = Color(0xFFF4EFF4),
    inverseSurface = Color(0xFF313033),
    inversePrimary = Color(0xFFD0BCFF),
    surfaceTint = Color(0xFF6750A4)
)

val DarkColorScheme = darkColorScheme(
    primary = Color(0xFFD0BCFF),
    onPrimary = Color(0xFF381E72),
    primaryContainer = Color(0xFF4F378B),
    onPrimaryContainer = Color(0xFFEADDFF),
    // ... other colors
)

// 2. Typography
val Typography = Typography(
    displayLarge = TextStyle(
        fontFamily = FontFamily.Default,
        fontWeight = FontWeight.Normal,
        fontSize = 57.sp,
        lineHeight = 64.sp,
        letterSpacing = (-0.25).sp,
    ),
    displayMedium = TextStyle(
        fontFamily = FontFamily.Default,
        fontWeight = FontWeight.Normal,
        fontSize = 45.sp,
        lineHeight = 52.sp,
        letterSpacing = 0.sp,
    ),
    // ... other text styles
)

// 3. Theme Composable
@Composable
fun MyAppTheme(
    darkTheme: Boolean = isSystemInDarkTheme(),
    dynamicColor: Boolean = true,
    content: @Composable () -> Unit
) {
    val colorScheme = when {
        dynamicColor && Build.VERSION.SDK_INT >= Build.VERSION_CODES.S -> {
            val context = LocalContext.current
            if (darkTheme) dynamicDarkColorScheme(context) else dynamicLightColorScheme(context)
        }
        darkTheme -> DarkColorScheme
        else -> LightColorScheme
    }

    MaterialTheme(
        colorScheme = colorScheme,
        typography = Typography,
        content = content
    )
}
```

### **Ngày 3-4: Material Components**
```kotlin
// 1. App Bars
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun MaterialAppBars() {
    var selectedItem by remember { mutableStateOf(0) }
    val items = listOf("Home", "Search", "Profile")
    
    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("My App") },
                navigationIcon = {
                    IconButton(onClick = { /* Handle navigation */ }) {
                        Icon(Icons.Default.Menu, contentDescription = "Menu")
                    }
                },
                actions = {
                    IconButton(onClick = { /* Handle search */ }) {
                        Icon(Icons.Default.Search, contentDescription = "Search")
                    }
                    IconButton(onClick = { /* Handle more */ }) {
                        Icon(Icons.Default.MoreVert, contentDescription = "More")
                    }
                }
            )
        },
        bottomBar = {
            NavigationBar {
                items.forEachIndexed { index, item ->
                    NavigationBarItem(
                        icon = { 
                            Icon(
                                when(index) {
                                    0 -> Icons.Default.Home
                                    1 -> Icons.Default.Search
                                    else -> Icons.Default.Person
                                },
                                contentDescription = item
                            )
                        },
                        label = { Text(item) },
                        selected = selectedItem == index,
                        onClick = { selectedItem = index }
                    )
                }
            }
        },
        floatingActionButton = {
            FloatingActionButton(
                onClick = { /* Handle FAB click */ }
            ) {
                Icon(Icons.Default.Add, contentDescription = "Add")
            }
        }
    ) { paddingValues ->
        // Screen content
        Box(
            modifier = Modifier
                .fillMaxSize()
                .padding(paddingValues),
            contentAlignment = Alignment.Center
        ) {
            Text("Screen ${items[selectedItem]}")
        }
    }
}

// 2. Cards và Surfaces
@Composable
fun MaterialCards() {
    LazyColumn(
        contentPadding = PaddingValues(16.dp),
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        item {
            // Elevated Card
            ElevatedCard(
                modifier = Modifier.fillMaxWidth(),
                elevation = CardDefaults.elevatedCardElevation(defaultElevation = 6.dp)
            ) {
                Column(modifier = Modifier.padding(16.dp)) {
                    Text(
                        text = "Elevated Card",
                        style = MaterialTheme.typography.headlineSmall
                    )
                    Text(
                        text = "This is an elevated card with shadow.",
                        style = MaterialTheme.typography.bodyMedium
                    )
                }
            }
        }
        
        item {
            // Outlined Card
            OutlinedCard(
                modifier = Modifier.fillMaxWidth(),
                border = BorderStroke(2.dp, MaterialTheme.colorScheme.outline)
            ) {
                Column(modifier = Modifier.padding(16.dp)) {
                    Text(
                        text = "Outlined Card",
                        style = MaterialTheme.typography.headlineSmall
                    )
                    Text(
                        text = "This is an outlined card with border.",
                        style = MaterialTheme.typography.bodyMedium
                    )
                }
            }
        }
        
        item {
            // Filled Card
            Card(
                modifier = Modifier.fillMaxWidth(),
                colors = CardDefaults.cardColors(
                    containerColor = MaterialTheme.colorScheme.surfaceVariant
                )
            ) {
                Column(modifier = Modifier.padding(16.dp)) {
                    Text(
                        text = "Filled Card",
                        style = MaterialTheme.typography.headlineSmall
                    )
                    Text(
                        text = "This is a filled card with background color.",
                        style = MaterialTheme.typography.bodyMedium
                    )
                }
            }
        }
    }
}
```

**Dự án tuần 5: Banking App UI**
- Implement complete Material 3 theme
- Dashboard với cards
- Transaction list
- Account details

---

## ⚡ **TUẦN 6: SIDE EFFECTS & ADVANCED STATE**

### **Ngày 1-3: Side Effects**
```kotlin
// 1. LaunchedEffect
@Composable
fun UserProfileScreen(userId: String) {
    var user by remember { mutableStateOf<User?>(null) }
    var isLoading by remember { mutableStateOf(true) }
    var error by remember { mutableStateOf<String?>(null) }
    
    LaunchedEffect(userId) {
        isLoading = true
        error = null
        try {
            user = fetchUser(userId)
        } catch (e: Exception) {
            error = e.message
        } finally {
            isLoading = false
        }
    }
    
    Box(modifier = Modifier.fillMaxSize()) {
        when {
            isLoading -> {
                CircularProgressIndicator(
                    modifier = Modifier.align(Alignment.Center)
                )
            }
            error != null -> {
                Column(
                    modifier = Modifier.align(Alignment.Center),
                    horizontalAlignment = Alignment.CenterHorizontally
                ) {
                    Text("Error: $error")
                    Button(
                        onClick = { 
                            // Trigger recomposition to retry
                            isLoading = true
                        }
                    ) {
                        Text("Retry")
                    }
                }
            }
            user != null -> {
                UserProfile(user!!)
            }
        }
    }
}

// 2. DisposableEffect
@Composable
fun TimerScreen() {
    var seconds by remember { mutableStateOf(0) }
    var isRunning by remember { mutableStateOf(false) }
    
    DisposableEffect(isRunning) {
        val timer = if (isRunning) {
            Timer().apply {
                scheduleAtFixedRate(object : TimerTask() {
                    override fun run() {
                        seconds++
                    }
                }, 0, 1000)
            }
        } else null
        
        onDispose {
            timer?.cancel()
        }
    }
    
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text(
            text = formatTime(seconds),
            style = MaterialTheme.typography.displayLarge
        )
        
        Row {
            Button(
                onClick = { isRunning = !isRunning }
            ) {
                Text(if (isRunning) "Pause" else "Start")
            }
            
            Spacer(modifier = Modifier.width(16.dp))
            
            Button(
                onClick = { 
                    seconds = 0
                    isRunning = false
                }
            ) {
                Text("Reset")
            }
        }
    }
}

// 3. SideEffect
@Composable
fun AnalyticsScreen() {
    val analyticsService = remember { AnalyticsService() }
    var screenName by remember { mutableStateOf("Home") }
    
    SideEffect {
        // This runs after every successful recomposition
        analyticsService.trackScreenView(screenName)
    }
    
    Column {
        Text("Current screen: $screenName")
        
        Button(onClick = { screenName = "Profile" }) {
            Text("Go to Profile")
        }
        
        Button(onClick = { screenName = "Settings" }) {
            Text("Go to Settings")
        }
    }
}

// 4. derivedStateOf
@Composable
fun SearchScreen() {
    var searchQuery by remember { mutableStateOf("") }
    val allItems = remember { generateLargeItemList() }
    
    // derivedStateOf prevents unnecessary recomposition of filtering
    val filteredItems by remember {
        derivedStateOf {
            if (searchQuery.isBlank()) {
                allItems
            } else {
                allItems.filter { 
                    it.name.contains(searchQuery, ignoreCase = true) 
                }
            }
        }
    }
    
    Column {
        TextField(
            value = searchQuery,
            onValueChange = { searchQuery = it },
            label = { Text("Search") },
            modifier = Modifier.fillMaxWidth()
        )
        
        LazyColumn {
            items(filteredItems) { item ->
                Text(item.name)
            }
        }
    }
}
```

### **Ngày 4-7: ViewModel Integration**
```kotlin
// 1. ViewModel
class UserViewModel : ViewModel() {
    private val _uiState = MutableStateFlow(UserUiState())
    val uiState = _uiState.asStateFlow()
    
    private val userRepository = UserRepository()
    
    fun loadUser(userId: String) {
        viewModelScope.launch {
            _uiState.value = _uiState.value.copy(isLoading = true)
            try {
                val user = userRepository.getUser(userId)
                _uiState.value = _uiState.value.copy(
                    user = user,
                    isLoading = false,
                    error = null
                )
            } catch (e: Exception) {
                _uiState.value = _uiState.value.copy(
                    isLoading = false,
                    error = e.message
                )
            }
        }
    }
    
    fun updateUser(user: User) {
        viewModelScope.launch {
            try {
                userRepository.updateUser(user)
                _uiState.value = _uiState.value.copy(user = user)
            } catch (e: Exception) {
                _uiState.value = _uiState.value.copy(error = e.message)
            }
        }
    }
}

data class UserUiState(
    val user: User? = null,
    val isLoading: Boolean = false,
    val error: String? = null
)

// 2. Screen với ViewModel
@Composable
fun UserScreen(
    userId: String,
    viewModel: UserViewModel = hiltViewModel()
) {
    val uiState by viewModel.uiState.collectAsState()
    
    LaunchedEffect(userId) {
        viewModel.loadUser(userId)
    }
    
    when {
        uiState.isLoading -> {
            LoadingScreen()
        }
        uiState.error != null -> {
            ErrorScreen(
                error = uiState.error!!,
                onRetry = { viewModel.loadUser(userId) }
            )
        }
        uiState.user != null -> {
            UserContent(
                user = uiState.user!!,
                onUserUpdate = viewModel::updateUser
            )
        }
    }
}

// 3. Repository Pattern
interface UserRepository {
    suspend fun getUser(userId: String): User
    suspend fun updateUser(user: User)
    suspend fun getUsers(): List<User>
}

class UserRepositoryImpl(
    private val apiService: ApiService,
    private val localDataSource: UserDao
) : UserRepository {
    
    override suspend fun getUser(userId: String): User {
        return try {
            val remoteUser = apiService.getUser(userId)
            localDataSource.insertUser(remoteUser)
            remoteUser
        } catch (e: Exception) {
            localDataSource.getUser(userId) ?: throw e
        }
    }
    
    override suspend fun updateUser(user: User) {
        apiService.updateUser(user)
        localDataSource.insertUser(user)
    }
    
    override suspend fun getUsers(): List<User> {
        return try {
            val remoteUsers = apiService.getUsers()
            localDataSource.insertUsers(remoteUsers)
            remoteUsers
        } catch (e: Exception) {
            localDataSource.getAllUsers()
        }
    }
}
```

**Dự án tuần 6: Social Media App**
- User profiles với API calls
- Real-time updates
- Error handling
- Loading states

---

# 📖 **RESOURCES VÀ PRACTICE**

## 📚 **Sách và Tài Liệu**
- **Official Compose Documentation**
- **Jetpack Compose by Tutorials** (raywenderlich)
- **Android UI Development with Jetpack Compose**

## 🎓 **Courses Online**
- **Android Basics with Compose** (Google)
- **Jetpack Compose for Android Developers** (Udacity)
- **Complete Android Jetpack Compose** (Udemy)

## 💻 **Practice Projects**
1. **Calculator App** (Tuần 1-2)
2. **Todo List** (Tuần 3)
3. **News Reader** (Tuần 4)
4. **Weather App** (Tuần 5-6)
5. **E-commerce App** (Tuần 7-8)

## 🔧 **Tools Setup**
```kotlin
// build.gradle.kts (Module: app)
android {
    compileSdk 34
    
    defaultConfig {
        minSdk 24
        targetSdk 34
    }
    
    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_1_8
        targetCompatibility = JavaVersion.VERSION_1_8
    }
    
    kotlinOptions {
        jvmTarget = "1.8"
    }
    
    buildFeatures {
        compose = true
    }
    
    composeOptions {
        kotlinCompilerExtensionVersion = "1.5.8"
    }
}

dependencies {
    val composeBom = platform("androidx.compose:compose-bom:2024.02.00")
    implementation(composeBom)
    
    implementation("androidx.compose.ui:ui")
    implementation("androidx.compose.ui:ui-tooling-preview")
    implementation("androidx.compose.material3:material3")
    implementation("androidx.activity:activity-compose:1.8.2")
    implementation("androidx.navigation:navigation-compose:2.7.6")
    implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.7.0")
    
    debugImplementation("androidx.compose.ui:ui-tooling")
    debugImplementation("androidx.compose.ui:ui-test-manifest")
}
```

---

# 🎯 **MILESTONE CHECKPOINTS**

## ✅ **Tuần 4 Checkpoint**
- [ ] Tạo được UI cơ bản với Compose
- [ ] Hiểu state management và hoisting
- [ ] Navigation giữa các screens
- [ ] LazyColumn với dynamic data

## ✅ **Tuần 8 Checkpoint**
- [ ] Material 3 theming hoàn chỉnh
- [ ] Side effects và ViewModel integration
- [ ] Animation cơ bản
- [ ] Error handling và loading states

## ✅ **Tuần 12 Checkpoint**
- [ ] Architecture patterns (MVVM)
- [ ] Testing Compose UI
- [ ] Performance optimization
- [ ] Custom components

## ✅ **Tuần 16 Checkpoint**
- [ ] Production-ready app
- [ ] Advanced animations
- [ ] Custom layouts
- [ ] Deployment knowledge

---

# 📝 **DAILY STUDY PLAN**

## ⏰ **Thời gian học: 3-4 giờ/ngày**

### **Buổi sáng (1.5 giờ)**
- Đọc documentation
- Xem tutorials
- Ghi chú concepts mới

### **Buổi chiều (1.5 giờ)**
- Code along tutorials
- Practice với examples
- Thử nghiệm với code

### **Buổi tối (1 giờ)**
- Làm dự án cá nhân
- Review code đã viết
- Chuẩn bị cho ngày mai

---

**Chúc bạn học tập hiệu quả và thành công với Jetpack Compose! 🚀** 