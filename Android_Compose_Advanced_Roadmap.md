# 🚀 JETPACK COMPOSE ADVANCED ROADMAP
## PHASE 3 & 4: ADVANCED TO EXPERT (Tuần 9-16)

---

# 🏗️ **PHASE 3: ADVANCED (Tuần 9-12)**

## 🎬 **TUẦN 7: ANIMATIONS & TRANSITIONS**

### **Ngày 1-2: Basic Animations**
```kotlin
// 1. animate*AsState APIs
@Composable
fun BasicAnimations() {
    var expanded by remember { mutableStateOf(false) }
    
    // Size animation
    val size by animateDpAsState(
        targetValue = if (expanded) 200.dp else 100.dp,
        animationSpec = spring(
            dampingRatio = Spring.DampingRatioMediumBouncy,
            stiffness = Spring.StiffnessLow
        ),
        label = "size animation"
    )
    
    // Color animation
    val color by animateColorAsState(
        targetValue = if (expanded) Color.Red else Color.Blue,
        animationSpec = tween(
            durationMillis = 1000,
            easing = FastOutSlowInEasing
        ),
        label = "color animation"
    )
    
    // Float animation
    val alpha by animateFloatAsState(
        targetValue = if (expanded) 1f else 0.5f,
        animationSpec = repeatable(
            iterations = 3,
            animation = tween(500),
            repeatMode = RepeatMode.Reverse
        ),
        label = "alpha animation"
    )
    
    Column(
        horizontalAlignment = Alignment.CenterHorizontally,
        modifier = Modifier.padding(16.dp)
    ) {
        Box(
            modifier = Modifier
                .size(size)
                .background(color, CircleShape)
                .alpha(alpha)
                .clickable { expanded = !expanded }
        )
        
        Spacer(modifier = Modifier.height(16.dp))
        
        Button(onClick = { expanded = !expanded }) {
            Text(if (expanded) "Collapse" else "Expand")
        }
    }
}

// 2. AnimatedVisibility
@Composable
fun VisibilityAnimations() {
    var visible by remember { mutableStateOf(true) }
    
    Column {
        Button(onClick = { visible = !visible }) {
            Text(if (visible) "Hide" else "Show")
        }
        
        Spacer(modifier = Modifier.height(16.dp))
        
        // Slide animation
        AnimatedVisibility(
            visible = visible,
            enter = slideInVertically(
                initialOffsetY = { -40 }
            ) + fadeIn(
                animationSpec = tween(500)
            ),
            exit = slideOutVertically(
                targetOffsetY = { -40 }
            ) + fadeOut(
                animationSpec = tween(500)
            )
        ) {
            Card(
                modifier = Modifier
                    .fillMaxWidth()
                    .height(100.dp),
                colors = CardDefaults.cardColors(
                    containerColor = MaterialTheme.colorScheme.primaryContainer
                )
            ) {
                Box(
                    modifier = Modifier.fillMaxSize(),
                    contentAlignment = Alignment.Center
                ) {
                    Text("Animated Content")
                }
            }
        }
        
        Spacer(modifier = Modifier.height(16.dp))
        
        // Scale animation
        AnimatedVisibility(
            visible = visible,
            enter = scaleIn(
                animationSpec = spring(
                    dampingRatio = Spring.DampingRatioMediumBouncy
                )
            ),
            exit = scaleOut(
                animationSpec = tween(300)
            )
        ) {
            FloatingActionButton(
                onClick = { /* Handle click */ }
            ) {
                Icon(Icons.Default.Add, contentDescription = "Add")
            }
        }
    }
}

// 3. AnimatedContent
@Composable
fun ContentTransitions() {
    var currentPage by remember { mutableStateOf(0) }
    val pages = listOf("Page 1", "Page 2", "Page 3")
    
    Column {
        Row {
            Button(
                onClick = { if (currentPage > 0) currentPage-- },
                enabled = currentPage > 0
            ) {
                Text("Previous")
            }
            
            Spacer(modifier = Modifier.width(16.dp))
            
            Button(
                onClick = { if (currentPage < pages.lastIndex) currentPage++ },
                enabled = currentPage < pages.lastIndex
            ) {
                Text("Next")
            }
        }
        
        Spacer(modifier = Modifier.height(16.dp))
        
        AnimatedContent(
            targetState = currentPage,
            transitionSpec = {
                if (targetState > initialState) {
                    slideInHorizontally { width -> width } + fadeIn() togetherWith
                            slideOutHorizontally { width -> -width } + fadeOut()
                } else {
                    slideInHorizontally { width -> -width } + fadeIn() togetherWith
                            slideOutHorizontally { width -> width } + fadeOut()
                }.using(
                    SizeTransform(clip = false)
                )
            },
            label = "page transition"
        ) { page ->
            Card(
                modifier = Modifier
                    .fillMaxWidth()
                    .height(200.dp),
                colors = CardDefaults.cardColors(
                    containerColor = when (page) {
                        0 -> MaterialTheme.colorScheme.primaryContainer
                        1 -> MaterialTheme.colorScheme.secondaryContainer
                        else -> MaterialTheme.colorScheme.tertiaryContainer
                    }
                )
            ) {
                Box(
                    modifier = Modifier.fillMaxSize(),
                    contentAlignment = Alignment.Center
                ) {
                    Text(
                        text = pages[page],
                        style = MaterialTheme.typography.headlineMedium
                    )
                }
            }
        }
    }
}
```

### **Ngày 3-4: Advanced Animations**
```kotlin
// 1. Custom Transition
@Composable
fun CustomTransition() {
    var selected by remember { mutableStateOf(false) }
    
    val transition = updateTransition(
        targetState = selected,
        label = "selection transition"
    )
    
    val size by transition.animateDp(
        transitionSpec = {
            if (false isTransitioningTo true) {
                spring(stiffness = Spring.StiffnessLow)
            } else {
                tween(300)
            }
        },
        label = "size"
    ) { isSelected ->
        if (isSelected) 150.dp else 100.dp
    }
    
    val color by transition.animateColor(
        transitionSpec = { tween(500) },
        label = "color"
    ) { isSelected ->
        if (isSelected) Color.Red else Color.Blue
    }
    
    val borderWidth by transition.animateDp(
        transitionSpec = { tween(300) },
        label = "border"
    ) { isSelected ->
        if (isSelected) 4.dp else 1.dp
    }
    
    Box(
        modifier = Modifier
            .size(size)
            .border(borderWidth, color, CircleShape)
            .background(color.copy(alpha = 0.3f), CircleShape)
            .clickable { selected = !selected },
        contentAlignment = Alignment.Center
    ) {
        Text(
            text = if (selected) "Selected" else "Select",
            color = color
        )
    }
}

// 2. Infinite Animation
@Composable
fun InfiniteAnimations() {
    // Rotating animation
    val infiniteTransition = rememberInfiniteTransition(label = "infinite transition")
    
    val rotation by infiniteTransition.animateFloat(
        initialValue = 0f,
        targetValue = 360f,
        animationSpec = infiniteRepeatable(
            animation = tween(2000, easing = LinearEasing),
            repeatMode = RepeatMode.Restart
        ),
        label = "rotation"
    )
    
    // Pulsing animation
    val scale by infiniteTransition.animateFloat(
        initialValue = 0.8f,
        targetValue = 1.2f,
        animationSpec = infiniteRepeatable(
            animation = tween(1000, easing = FastOutSlowInEasing),
            repeatMode = RepeatMode.Reverse
        ),
        label = "scale"
    )
    
    // Color cycling
    val color by infiniteTransition.animateColor(
        initialValue = Color.Red,
        targetValue = Color.Blue,
        animationSpec = infiniteRepeatable(
            animation = tween(3000),
            repeatMode = RepeatMode.Reverse
        ),
        label = "color"
    )
    
    Column(
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.spacedBy(32.dp)
    ) {
        // Rotating icon
        Icon(
            Icons.Default.Refresh,
            contentDescription = null,
            modifier = Modifier
                .size(48.dp)
                .rotate(rotation),
            tint = MaterialTheme.colorScheme.primary
        )
        
        // Pulsing circle
        Box(
            modifier = Modifier
                .size(80.dp)
                .scale(scale)
                .background(color, CircleShape)
        )
        
        // Loading dots
        LoadingDots()
    }
}

@Composable
fun LoadingDots() {
    val infiniteTransition = rememberInfiniteTransition(label = "loading dots")
    
    Row(
        horizontalArrangement = Arrangement.spacedBy(8.dp)
    ) {
        repeat(3) { index ->
            val alpha by infiniteTransition.animateFloat(
                initialValue = 0.3f,
                targetValue = 1f,
                animationSpec = infiniteRepeatable(
                    animation = tween(600),
                    repeatMode = RepeatMode.Reverse,
                    initialStartOffset = StartOffset(index * 200)
                ),
                label = "dot $index alpha"
            )
            
            Box(
                modifier = Modifier
                    .size(12.dp)
                    .alpha(alpha)
                    .background(MaterialTheme.colorScheme.primary, CircleShape)
            )
        }
    }
}

// 3. Gesture-based Animation
@Composable
fun SwipeToDelete() {
    var offsetX by remember { mutableStateOf(0f) }
    val threshold = 200f
    
    val animatedOffset by animateFloatAsState(
        targetValue = offsetX,
        animationSpec = spring(
            dampingRatio = Spring.DampingRatioMediumBouncy,
            stiffness = Spring.StiffnessLow
        ),
        label = "swipe offset"
    )
    
    Box(
        modifier = Modifier
            .fillMaxWidth()
            .height(80.dp)
            .background(Color.Red.copy(alpha = 0.1f))
    ) {
        // Delete background
        if (abs(animatedOffset) > threshold * 0.5f) {
            Row(
                modifier = Modifier
                    .align(
                        if (animatedOffset > 0) Alignment.CenterStart else Alignment.CenterEnd
                    )
                    .padding(16.dp),
                verticalAlignment = Alignment.CenterVertically
            ) {
                Icon(
                    Icons.Default.Delete,
                    contentDescription = "Delete",
                    tint = Color.Red
                )
                Text("Delete", color = Color.Red)
            }
        }
        
        // Item content
        Card(
            modifier = Modifier
                .fillMaxSize()
                .offset { IntOffset(animatedOffset.roundToInt(), 0) }
                .pointerInput(Unit) {
                    detectHorizontalDragGestures(
                        onDragEnd = {
                            if (abs(offsetX) > threshold) {
                                // Trigger delete action
                                offsetX = if (offsetX > 0) size.width.toFloat() else -size.width.toFloat()
                            } else {
                                offsetX = 0f
                            }
                        }
                    ) { _, dragAmount ->
                        offsetX += dragAmount
                    }
                },
            elevation = CardDefaults.cardElevation(defaultElevation = 4.dp)
        ) {
            Row(
                modifier = Modifier
                    .fillMaxSize()
                    .padding(16.dp),
                verticalAlignment = Alignment.CenterVertically
            ) {
                Text("Swipe me to delete")
            }
        }
    }
}
```

### **Ngày 5-7: MotionLayout & Complex Animations**
```kotlin
// 1. Coordinated Animations
@Composable
fun CoordinatedAnimations() {
    var expanded by remember { mutableStateOf(false) }
    
    val progress by animateFloatAsState(
        targetValue = if (expanded) 1f else 0f,
        animationSpec = spring(
            dampingRatio = Spring.DampingRatioMediumBouncy,
            stiffness = Spring.StiffnessLow
        ),
        label = "progress"
    )
    
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp)
    ) {
        // Header that moves up
        Box(
            modifier = Modifier
                .fillMaxWidth()
                .height(lerp(200.dp, 80.dp, progress))
                .background(
                    lerp(
                        MaterialTheme.colorScheme.primaryContainer,
                        MaterialTheme.colorScheme.primary,
                        progress
                    ),
                    RoundedCornerShape(lerp(0.dp, 16.dp, progress))
                )
                .clickable { expanded = !expanded },
            contentAlignment = Alignment.Center
        ) {
            Text(
                text = "Header",
                color = lerp(
                    MaterialTheme.colorScheme.onPrimaryContainer,
                    MaterialTheme.colorScheme.onPrimary,
                    progress
                ),
                fontSize = lerp(24.sp, 18.sp, progress)
            )
        }
        
        Spacer(modifier = Modifier.height(16.dp))
        
        // Content that fades in
        AnimatedVisibility(
            visible = expanded,
            enter = fadeIn(tween(500)) + slideInVertically(),
            exit = fadeOut(tween(300)) + slideOutVertically()
        ) {
            LazyColumn {
                items(10) { index ->
                    Card(
                        modifier = Modifier
                            .fillMaxWidth()
                            .padding(vertical = 4.dp),
                        elevation = CardDefaults.cardElevation(defaultElevation = 2.dp)
                    ) {
                        Text(
                            text = "Item ${index + 1}",
                            modifier = Modifier.padding(16.dp)
                        )
                    }
                }
            }
        }
    }
}

// 2. Shared Element Transition (Concept)
@Composable
fun SharedElementTransition() {
    var showDetail by remember { mutableStateOf(false) }
    
    Box(modifier = Modifier.fillMaxSize()) {
        AnimatedVisibility(
            visible = !showDetail,
            exit = fadeOut() + scaleOut(targetScale = 0.8f)
        ) {
            LazyVerticalGrid(
                columns = GridCells.Fixed(2),
                contentPadding = PaddingValues(16.dp),
                horizontalArrangement = Arrangement.spacedBy(8.dp),
                verticalArrangement = Arrangement.spacedBy(8.dp)
            ) {
                items(10) { index ->
                    Card(
                        modifier = Modifier
                            .aspectRatio(1f)
                            .clickable { showDetail = true },
                        elevation = CardDefaults.cardElevation(defaultElevation = 4.dp)
                    ) {
                        Box(
                            modifier = Modifier
                                .fillMaxSize()
                                .background(
                                    brush = Brush.linearGradient(
                                        listOf(
                                            Color.Blue.copy(alpha = 0.7f),
                                            Color.Purple.copy(alpha = 0.7f)
                                        )
                                    )
                                ),
                            contentAlignment = Alignment.Center
                        ) {
                            Text(
                                text = "Item $index",
                                color = Color.White,
                                fontWeight = FontWeight.Bold
                            )
                        }
                    }
                }
            }
        }
        
        AnimatedVisibility(
            visible = showDetail,
            enter = fadeIn() + scaleIn(initialScale = 0.8f),
            exit = fadeOut() + scaleOut(targetScale = 0.8f)
        ) {
            DetailView(onClose = { showDetail = false })
        }
    }
}

@Composable
fun DetailView(onClose: () -> Unit) {
    Box(
        modifier = Modifier
            .fillMaxSize()
            .background(MaterialTheme.colorScheme.surface)
    ) {
        Column {
            // Top bar
            Row(
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(16.dp),
                verticalAlignment = Alignment.CenterVertically
            ) {
                IconButton(onClick = onClose) {
                    Icon(Icons.Default.ArrowBack, contentDescription = "Back")
                }
                Text(
                    text = "Detail View",
                    style = MaterialTheme.typography.headlineSmall,
                    modifier = Modifier.weight(1f)
                )
            }
            
            // Content
            Box(
                modifier = Modifier
                    .fillMaxWidth()
                    .height(300.dp)
                    .padding(16.dp)
                    .background(
                        brush = Brush.linearGradient(
                            listOf(
                                Color.Blue.copy(alpha = 0.7f),
                                Color.Purple.copy(alpha = 0.7f)
                            )
                        ),
                        shape = RoundedCornerShape(16.dp)
                    ),
                contentAlignment = Alignment.Center
            ) {
                Text(
                    text = "Detailed Content",
                    color = Color.White,
                    style = MaterialTheme.typography.headlineMedium
                )
            }
        }
    }
}
```

**Dự án tuần 7: Animated Dashboard**
- Card expansions
- Page transitions
- Loading animations
- Interactive gestures

---

## 🏛️ **TUẦN 8: ARCHITECTURE PATTERNS**

### **Ngày 1-3: MVVM với Compose**
```kotlin
// 1. Domain Layer
data class Product(
    val id: String,
    val name: String,
    val price: Double,
    val imageUrl: String,
    val description: String,
    val category: String,
    val inStock: Boolean
)

interface ProductRepository {
    suspend fun getProducts(): Result<List<Product>>
    suspend fun getProduct(id: String): Result<Product>
    suspend fun searchProducts(query: String): Result<List<Product>>
    fun getFavorites(): Flow<List<Product>>
    suspend fun addToFavorites(product: Product)
    suspend fun removeFromFavorites(productId: String)
}

// 2. Use Cases
class GetProductsUseCase(
    private val repository: ProductRepository
) {
    suspend operator fun invoke(): Result<List<Product>> {
        return repository.getProducts()
    }
}

class SearchProductsUseCase(
    private val repository: ProductRepository
) {
    suspend operator fun invoke(query: String): Result<List<Product>> {
        return repository.searchProducts(query)
    }
}

class ToggleFavoriteUseCase(
    private val repository: ProductRepository
) {
    suspend operator fun invoke(product: Product, isFavorite: Boolean) {
        if (isFavorite) {
            repository.addToFavorites(product)
        } else {
            repository.removeFromFavorites(product.id)
        }
    }
}

// 3. ViewModel
data class ProductsUiState(
    val products: List<Product> = emptyList(),
    val favorites: List<Product> = emptyList(),
    val isLoading: Boolean = false,
    val error: String? = null,
    val searchQuery: String = "",
    val selectedCategory: String = "All"
)

@HiltViewModel
class ProductsViewModel @Inject constructor(
    private val getProductsUseCase: GetProductsUseCase,
    private val searchProductsUseCase: SearchProductsUseCase,
    private val toggleFavoriteUseCase: ToggleFavoriteUseCase,
    private val repository: ProductRepository
) : ViewModel() {
    
    private val _uiState = MutableStateFlow(ProductsUiState())
    val uiState = _uiState.asStateFlow()
    
    private val searchDebounce = MutableSharedFlow<String>()
    
    init {
        loadProducts()
        observeFavorites()
        setupSearchDebounce()
    }
    
    private fun loadProducts() {
        viewModelScope.launch {
            _uiState.value = _uiState.value.copy(isLoading = true)
            
            getProductsUseCase().fold(
                onSuccess = { products ->
                    _uiState.value = _uiState.value.copy(
                        products = products,
                        isLoading = false,
                        error = null
                    )
                },
                onFailure = { exception ->
                    _uiState.value = _uiState.value.copy(
                        isLoading = false,
                        error = exception.message
                    )
                }
            )
        }
    }
    
    private fun observeFavorites() {
        viewModelScope.launch {
            repository.getFavorites().collect { favorites ->
                _uiState.value = _uiState.value.copy(favorites = favorites)
            }
        }
    }
    
    private fun setupSearchDebounce() {
        viewModelScope.launch {
            searchDebounce
                .debounce(300)
                .distinctUntilChanged()
                .collect { query ->
                    performSearch(query)
                }
        }
    }
    
    fun onSearchQueryChanged(query: String) {
        _uiState.value = _uiState.value.copy(searchQuery = query)
        viewModelScope.launch {
            searchDebounce.emit(query)
        }
    }
    
    private suspend fun performSearch(query: String) {
        if (query.isBlank()) {
            loadProducts()
            return
        }
        
        _uiState.value = _uiState.value.copy(isLoading = true)
        
        searchProductsUseCase(query).fold(
            onSuccess = { products ->
                _uiState.value = _uiState.value.copy(
                    products = products,
                    isLoading = false,
                    error = null
                )
            },
            onFailure = { exception ->
                _uiState.value = _uiState.value.copy(
                    isLoading = false,
                    error = exception.message
                )
            }
        )
    }
    
    fun onCategorySelected(category: String) {
        _uiState.value = _uiState.value.copy(selectedCategory = category)
        // Filter products by category
    }
    
    fun onToggleFavorite(product: Product) {
        viewModelScope.launch {
            val isFavorite = _uiState.value.favorites.any { it.id == product.id }
            toggleFavoriteUseCase(product, !isFavorite)
        }
    }
    
    fun onRetry() {
        loadProducts()
    }
}

// 4. Screen Composable
@Composable
fun ProductsScreen(
    onProductClick: (Product) -> Unit,
    viewModel: ProductsViewModel = hiltViewModel()
) {
    val uiState by viewModel.uiState.collectAsState()
    
    Column(modifier = Modifier.fillMaxSize()) {
        // Search bar
        SearchBar(
            query = uiState.searchQuery,
            onQueryChanged = viewModel::onSearchQueryChanged
        )
        
        // Category filter
        CategoryFilter(
            selectedCategory = uiState.selectedCategory,
            onCategorySelected = viewModel::onCategorySelected
        )
        
        // Products content
        when {
            uiState.isLoading -> {
                LoadingScreen()
            }
            uiState.error != null -> {
                ErrorScreen(
                    error = uiState.error!!,
                    onRetry = viewModel::onRetry
                )
            }
            uiState.products.isEmpty() -> {
                EmptyScreen()
            }
            else -> {
                ProductsList(
                    products = uiState.products,
                    favorites = uiState.favorites,
                    onProductClick = onProductClick,
                    onToggleFavorite = viewModel::onToggleFavorite
                )
            }
        }
    }
}

@Composable
fun ProductsList(
    products: List<Product>,
    favorites: List<Product>,
    onProductClick: (Product) -> Unit,
    onToggleFavorite: (Product) -> Unit
) {
    LazyVerticalGrid(
        columns = GridCells.Fixed(2),
        contentPadding = PaddingValues(16.dp),
        horizontalArrangement = Arrangement.spacedBy(8.dp),
        verticalArrangement = Arrangement.spacedBy(8.dp)
    ) {
        items(
            items = products,
            key = { it.id }
        ) { product ->
            val isFavorite = favorites.any { it.id == product.id }
            
            ProductCard(
                product = product,
                isFavorite = isFavorite,
                onClick = { onProductClick(product) },
                onToggleFavorite = { onToggleFavorite(product) }
            )
        }
    }
}
```

### **Ngày 4-7: Repository Pattern & Data Layer**
```kotlin
// 1. Data Sources
interface ProductRemoteDataSource {
    suspend fun getProducts(): List<ProductDto>
    suspend fun getProduct(id: String): ProductDto
    suspend fun searchProducts(query: String): List<ProductDto>
}

interface ProductLocalDataSource {
    suspend fun getProducts(): List<ProductEntity>
    suspend fun getProduct(id: String): ProductEntity?
    suspend fun insertProducts(products: List<ProductEntity>)
    suspend fun insertProduct(product: ProductEntity)
    suspend fun deleteProduct(id: String)
    suspend fun searchProducts(query: String): List<ProductEntity>
    fun getFavorites(): Flow<List<ProductEntity>>
    suspend fun addToFavorites(product: ProductEntity)
    suspend fun removeFromFavorites(productId: String)
}

// 2. Repository Implementation
@Singleton
class ProductRepositoryImpl @Inject constructor(
    private val remoteDataSource: ProductRemoteDataSource,
    private val localDataSource: ProductLocalDataSource,
    private val productMapper: ProductMapper,
    private val networkMonitor: NetworkMonitor
) : ProductRepository {
    
    override suspend fun getProducts(): Result<List<Product>> {
        return try {
            if (networkMonitor.isOnline()) {
                // Fetch from remote
                val remoteProducts = remoteDataSource.getProducts()
                val productEntities = remoteProducts.map { productMapper.dtoToEntity(it) }
                
                // Cache locally
                localDataSource.insertProducts(productEntities)
                
                // Return mapped domain objects
                val products = productEntities.map { productMapper.entityToDomain(it) }
                Result.success(products)
            } else {
                // Fallback to local data
                val localProducts = localDataSource.getProducts()
                val products = localProducts.map { productMapper.entityToDomain(it) }
                Result.success(products)
            }
        } catch (e: Exception) {
            // Try local data as fallback
            try {
                val localProducts = localDataSource.getProducts()
                val products = localProducts.map { productMapper.entityToDomain(it) }
                Result.success(products)
            } catch (localException: Exception) {
                Result.failure(e)
            }
        }
    }
    
    override suspend fun getProduct(id: String): Result<Product> {
        return try {
            val localProduct = localDataSource.getProduct(id)
            if (localProduct != null) {
                Result.success(productMapper.entityToDomain(localProduct))
            } else if (networkMonitor.isOnline()) {
                val remoteProduct = remoteDataSource.getProduct(id)
                val productEntity = productMapper.dtoToEntity(remoteProduct)
                localDataSource.insertProduct(productEntity)
                Result.success(productMapper.entityToDomain(productEntity))
            } else {
                Result.failure(Exception("Product not found"))
            }
        } catch (e: Exception) {
            Result.failure(e)
        }
    }
    
    override suspend fun searchProducts(query: String): Result<List<Product>> {
        return try {
            if (networkMonitor.isOnline()) {
                val remoteProducts = remoteDataSource.searchProducts(query)
                val products = remoteProducts.map { 
                    productMapper.entityToDomain(productMapper.dtoToEntity(it))
                }
                Result.success(products)
            } else {
                val localProducts = localDataSource.searchProducts(query)
                val products = localProducts.map { productMapper.entityToDomain(it) }
                Result.success(products)
            }
        } catch (e: Exception) {
            Result.failure(e)
        }
    }
    
    override fun getFavorites(): Flow<List<Product>> {
        return localDataSource.getFavorites().map { entities ->
            entities.map { productMapper.entityToDomain(it) }
        }
    }
    
    override suspend fun addToFavorites(product: Product) {
        val entity = productMapper.domainToEntity(product)
        localDataSource.addToFavorites(entity)
    }
    
    override suspend fun removeFromFavorites(productId: String) {
        localDataSource.removeFromFavorites(productId)
    }
}

// 3. Mappers
@Singleton
class ProductMapper @Inject constructor() {
    
    fun dtoToEntity(dto: ProductDto): ProductEntity {
        return ProductEntity(
            id = dto.id,
            name = dto.name,
            price = dto.price,
            imageUrl = dto.imageUrl,
            description = dto.description,
            category = dto.category,
            inStock = dto.inStock,
            lastUpdated = System.currentTimeMillis()
        )
    }
    
    fun entityToDomain(entity: ProductEntity): Product {
        return Product(
            id = entity.id,
            name = entity.name,
            price = entity.price,
            imageUrl = entity.imageUrl,
            description = entity.description,
            category = entity.category,
            inStock = entity.inStock
        )
    }
    
    fun domainToEntity(domain: Product): ProductEntity {
        return ProductEntity(
            id = domain.id,
            name = domain.name,
            price = domain.price,
            imageUrl = domain.imageUrl,
            description = domain.description,
            category = domain.category,
            inStock = domain.inStock,
            lastUpdated = System.currentTimeMillis()
        )
    }
}

// 4. Dependency Injection
@Module
@InstallIn(SingletonComponent::class)
abstract class RepositoryModule {
    
    @Binds
    abstract fun bindProductRepository(
        productRepositoryImpl: ProductRepositoryImpl
    ): ProductRepository
    
    @Binds
    abstract fun bindProductRemoteDataSource(
        productRemoteDataSourceImpl: ProductRemoteDataSourceImpl
    ): ProductRemoteDataSource
    
    @Binds
    abstract fun bindProductLocalDataSource(
        productLocalDataSourceImpl: ProductLocalDataSourceImpl
    ): ProductLocalDataSource
}

@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {
    
    @Provides
    @Singleton
    fun provideRetrofit(): Retrofit {
        return Retrofit.Builder()
            .baseUrl("https://api.example.com/")
            .addConverterFactory(GsonConverterFactory.create())
            .build()
    }
    
    @Provides
    @Singleton
    fun provideProductApiService(retrofit: Retrofit): ProductApiService {
        return retrofit.create(ProductApiService::class.java)
    }
}

@Module
@InstallIn(SingletonComponent::class)
object DatabaseModule {
    
    @Provides
    @Singleton
    fun provideAppDatabase(@ApplicationContext context: Context): AppDatabase {
        return Room.databaseBuilder(
            context,
            AppDatabase::class.java,
            "app_database"
        ).build()
    }
    
    @Provides
    fun provideProductDao(database: AppDatabase): ProductDao {
        return database.productDao()
    }
}
```

**Dự án tuần 8: E-commerce App**
- Complete MVVM architecture
- Repository pattern với caching
- Error handling
- Offline support

---

# 🚀 **PHASE 4: EXPERT (Tuần 13-16)**

## 🧪 **TUẦN 9: TESTING COMPOSE**

### **Ngày 1-3: UI Testing**
```kotlin
// 1. Basic Testing Setup
class LoginScreenTest {
    
    @get:Rule
    val composeTestRule = createComposeRule()
    
    @Test
    fun loginScreen_displaysAllElements() {
        composeTestRule.setContent {
            LoginScreen(
                onLoginClick = { _, _ -> },
                onSignUpClick = { }
            )
        }
        
        // Verify UI elements
        composeTestRule.onNodeWithText("Login").assertIsDisplayed()
        composeTestRule.onNodeWithText("Email").assertIsDisplayed()
        composeTestRule.onNodeWithText("Password").assertIsDisplayed()
        composeTestRule.onNodeWithContentDescription("Email input").assertIsDisplayed()
        composeTestRule.onNodeWithContentDescription("Password input").assertIsDisplayed()
        composeTestRule.onNodeWithText("Sign In").assertIsDisplayed()
        composeTestRule.onNodeWithText("Sign Up").assertIsDisplayed()
    }
    
    @Test
    fun loginScreen_handlesUserInput() {
        var email = ""
        var password = ""
        
        composeTestRule.setContent {
            LoginScreen(
                onLoginClick = { e, p -> 
                    email = e
                    password = p
                },
                onSignUpClick = { }
            )
        }
        
        // Input email
        composeTestRule.onNodeWithContentDescription("Email input")
            .performTextInput("test@example.com")
        
        // Input password
        composeTestRule.onNodeWithContentDescription("Password input")
            .performTextInput("password123")
        
        // Click login
        composeTestRule.onNodeWithText("Sign In").performClick()
        
        // Verify callback was called with correct values
        assertEquals("test@example.com", email)
        assertEquals("password123", password)
    }
    
    @Test
    fun loginScreen_showsValidationErrors() {
        composeTestRule.setContent {
            LoginScreen(
                onLoginClick = { _, _ -> },
                onSignUpClick = { }
            )
        }
        
        // Click login without entering data
        composeTestRule.onNodeWithText("Sign In").performClick()
        
        // Verify validation errors
        composeTestRule.onNodeWithText("Email is required").assertIsDisplayed()
        composeTestRule.onNodeWithText("Password is required").assertIsDisplayed()
    }
    
    @Test
    fun loginScreen_handlesLoadingState() {
        composeTestRule.setContent {
            var isLoading by remember { mutableStateOf(false) }
            
            Column {
                Button(onClick = { isLoading = true }) {
                    Text("Start Loading")
                }
                
                LoginScreen(
                    isLoading = isLoading,
                    onLoginClick = { _, _ -> },
                    onSignUpClick = { }
                )
            }
        }
        
        // Start loading
        composeTestRule.onNodeWithText("Start Loading").performClick()
        
        // Verify loading indicator
        composeTestRule.onNodeWithContentDescription("Loading").assertIsDisplayed()
        
        // Verify login button is disabled
        composeTestRule.onNodeWithText("Sign In").assertIsNotEnabled()
    }
}

// 2. Testing Lists
class ProductListTest {
    
    @get:Rule
    val composeTestRule = createComposeRule()
    
    private val testProducts = listOf(
        Product("1", "Product 1", 10.0, "", "Description 1", "Category A", true),
        Product("2", "Product 2", 20.0, "", "Description 2", "Category B", false),
        Product("3", "Product 3", 30.0, "", "Description 3", "Category A", true)
    )
    
    @Test
    fun productList_displaysAllProducts() {
        composeTestRule.setContent {
            ProductList(
                products = testProducts,
                onProductClick = { },
                onAddToCart = { }
            )
        }
        
        // Verify all products are displayed
        testProducts.forEach { product ->
            composeTestRule.onNodeWithText(product.name).assertIsDisplayed()
            composeTestRule.onNodeWithText("$${product.price}").assertIsDisplayed()
        }
    }
    
    @Test
    fun productList_handlesProductClick() {
        var clickedProduct: Product? = null
        
        composeTestRule.setContent {
            ProductList(
                products = testProducts,
                onProductClick = { clickedProduct = it },
                onAddToCart = { }
            )
        }
        
        // Click on first product
        composeTestRule.onNodeWithText("Product 1").performClick()
        
        // Verify callback
        assertEquals(testProducts[0], clickedProduct)
    }
    
    @Test
    fun productList_scrollsToItem() {
        // Create large list
        val largeProductList = (1..100).map { 
            Product("$it", "Product $it", it.toDouble(), "", "Description $it", "Category", true)
        }
        
        composeTestRule.setContent {
            ProductList(
                products = largeProductList,
                onProductClick = { },
                onAddToCart = { }
            )
        }
        
        // Item at bottom should not be visible initially
        composeTestRule.onNodeWithText("Product 100").assertDoesNotExist()
        
        // Scroll to bottom
        composeTestRule.onNodeWithText("Product 100").performScrollTo()
        
        // Now it should be visible
        composeTestRule.onNodeWithText("Product 100").assertIsDisplayed()
    }
}

// 3. Testing Navigation
class NavigationTest {
    
    @get:Rule
    val composeTestRule = createComposeRule()
    
    @Test
    fun navigation_navigatesToDetailScreen() {
        composeTestRule.setContent {
            val navController = rememberNavController()
            
            NavHost(
                navController = navController,
                startDestination = "home"
            ) {
                composable("home") {
                    HomeScreen(
                        onProductClick = { productId ->
                            navController.navigate("product/$productId")
                        }
                    )
                }
                composable("product/{productId}") { backStackEntry ->
                    val productId = backStackEntry.arguments?.getString("productId")
                    ProductDetailScreen(productId = productId)
                }
            }
        }
        
        // Click on product
        composeTestRule.onNodeWithText("Product 1").performClick()
        
        // Verify navigation to detail screen
        composeTestRule.onNodeWithText("Product Details").assertIsDisplayed()
    }
}
```

### **Ngày 4-7: Testing ViewModels & Integration**
```kotlin
// 1. ViewModel Testing
class ProductsViewModelTest {
    
    @get:Rule
    val mainDispatcherRule = MainDispatcherRule()
    
    private val mockRepository = mockk<ProductRepository>()
    private lateinit var viewModel: ProductsViewModel
    
    @Before
    fun setup() {
        viewModel = ProductsViewModel(mockRepository)
    }
    
    @Test
    fun `loadProducts updates ui state with success`() = runTest {
        // Given
        val expectedProducts = listOf(
            Product("1", "Product 1", 10.0, "", "Description", "Category", true)
        )
        coEvery { mockRepository.getProducts() } returns Result.success(expectedProducts)
        
        // When
        viewModel.loadProducts()
        
        // Then
        val uiState = viewModel.uiState.value
        assertEquals(expectedProducts, uiState.products)
        assertEquals(false, uiState.isLoading)
        assertEquals(null, uiState.error)
    }
    
    @Test
    fun `loadProducts updates ui state with error`() = runTest {
        // Given
        val errorMessage = "Network error"
        coEvery { mockRepository.getProducts() } returns Result.failure(Exception(errorMessage))
        
        // When
        viewModel.loadProducts()
        
        // Then
        val uiState = viewModel.uiState.value
        assertEquals(emptyList<Product>(), uiState.products)
        assertEquals(false, uiState.isLoading)
        assertEquals(errorMessage, uiState.error)
    }
    
    @Test
    fun `search debounces user input`() = runTest {
        // Given
        val searchQuery = "test"
        coEvery { mockRepository.searchProducts(searchQuery) } returns Result.success(emptyList())
        
        // When
        viewModel.onSearchQueryChanged(searchQuery)
        
        // Then
        assertEquals(searchQuery, viewModel.uiState.value.searchQuery)
        
        // Advance time to trigger debounce
        advanceTimeBy(400)
        
        // Verify search was performed
        coVerify { mockRepository.searchProducts(searchQuery) }
    }
}

// 2. Repository Testing
class ProductRepositoryTest {
    
    private val mockRemoteDataSource = mockk<ProductRemoteDataSource>()
    private val mockLocalDataSource = mockk<ProductLocalDataSource>()
    private val mockNetworkMonitor = mockk<NetworkMonitor>()
    private val mockMapper = mockk<ProductMapper>()
    
    private lateinit var repository: ProductRepositoryImpl
    
    @Before
    fun setup() {
        repository = ProductRepositoryImpl(
            mockRemoteDataSource,
            mockLocalDataSource,
            mockMapper,
            mockNetworkMonitor
        )
    }
    
    @Test
    fun `getProducts returns remote data when online`() = runTest {
        // Given
        val remoteProducts = listOf(ProductDto("1", "Product 1", 10.0))
        val entityProducts = listOf(ProductEntity("1", "Product 1", 10.0))
        val domainProducts = listOf(Product("1", "Product 1", 10.0, "", "", "", true))
        
        every { mockNetworkMonitor.isOnline() } returns true
        coEvery { mockRemoteDataSource.getProducts() } returns remoteProducts
        every { mockMapper.dtoToEntity(any()) } returns entityProducts[0]
        every { mockMapper.entityToDomain(any()) } returns domainProducts[0]
        coEvery { mockLocalDataSource.insertProducts(any()) } just Runs
        
        // When
        val result = repository.getProducts()
        
        // Then
        assertTrue(result.isSuccess)
        assertEquals(domainProducts, result.getOrNull())
        coVerify { mockLocalDataSource.insertProducts(entityProducts) }
    }
    
    @Test
    fun `getProducts returns local data when offline`() = runTest {
        // Given
        val localProducts = listOf(ProductEntity("1", "Product 1", 10.0))
        val domainProducts = listOf(Product("1", "Product 1", 10.0, "", "", "", true))
        
        every { mockNetworkMonitor.isOnline() } returns false
        coEvery { mockLocalDataSource.getProducts() } returns localProducts
        every { mockMapper.entityToDomain(any()) } returns domainProducts[0]
        
        // When
        val result = repository.getProducts()
        
        // Then
        assertTrue(result.isSuccess)
        assertEquals(domainProducts, result.getOrNull())
        coVerify(exactly = 0) { mockRemoteDataSource.getProducts() }
    }
}

// 3. End-to-End Testing
@HiltAndroidTest
class ProductsE2ETest {
    
    @get:Rule(order = 0)
    val hiltRule = HiltAndroidRule(this)
    
    @get:Rule(order = 1)
    val composeTestRule = createAndroidComposeRule<MainActivity>()
    
    @Before
    fun init() {
        hiltRule.inject()
    }
    
    @Test
    fun userCanSearchAndViewProductDetails() {
        // Start at home screen
        composeTestRule.onNodeWithText("Products").assertIsDisplayed()
        
        // Perform search
        composeTestRule.onNodeWithContentDescription("Search")
            .performTextInput("laptop")
        
        // Wait for search results
        composeTestRule.waitUntil(5000) {
            composeTestRule.onAllNodesWithText("Laptop").fetchSemanticsNodes().isNotEmpty()
        }
        
        // Click on first result
        composeTestRule.onNodeWithText("Laptop").performClick()
        
        // Verify detail screen
        composeTestRule.onNodeWithText("Product Details").assertIsDisplayed()
        composeTestRule.onNodeWithText("Add to Cart").assertIsDisplayed()
        
        // Add to cart
        composeTestRule.onNodeWithText("Add to Cart").performClick()
        
        // Verify success message
        composeTestRule.onNodeWithText("Added to cart").assertIsDisplayed()
    }
}
```

**Dự án tuần 9: Testing Suite**
- Unit tests cho ViewModels
- UI tests cho screens
- Integration tests
- E2E test scenarios

---

## ⚡ **TUẦN 10: PERFORMANCE OPTIMIZATION**

### **Ngày 1-3: Recomposition Optimization**
```kotlin
// 1. Stable vs Unstable Types
@Immutable
data class UserProfile(
    val id: String,
    val name: String,
    val email: String,
    val avatarUrl: String
)

@Stable
class UserViewModel {
    // Stable class implementation
}

// Unstable class (avoid this)
class BadUserData {
    var name: String = ""
    var email: String = ""
    // Mutable properties make this unstable
}

// 2. Using @Stable annotation
@Stable
interface UserActions {
    fun onNameChanged(name: String)
    fun onEmailChanged(email: String)
    fun onSave()
}

@Composable
fun UserForm(
    user: UserProfile,
    actions: UserActions // Stable interface
) {
    // This composable will only recompose when user changes
    Column {
        TextField(
            value = user.name,
            onValueChange = actions::onNameChanged
        )
        TextField(
            value = user.email,
            onValueChange = actions::onEmailChanged
        )
        Button(onClick = actions::onSave) {
            Text("Save")
        }
    }
}

// 3. Optimizing with remember
@Composable
fun ExpensiveCalculationExample() {
    var input by remember { mutableStateOf("") }
    
    // ❌ Bad: Recalculates on every recomposition
    val expensiveResult = performExpensiveCalculation(input)
    
    // ✅ Good: Only recalculates when input changes
    val optimizedResult = remember(input) {
        performExpensiveCalculation(input)
    }
    
    // ✅ Even better: Use derivedStateOf for derived state
    val derivedResult by remember {
        derivedStateOf {
            performExpensiveCalculation(input)
        }
    }
    
    Column {
        TextField(
            value = input,
            onValueChange = { input = it }
        )
        Text("Result: $derivedResult")
    }
}

// 4. LazyColumn Optimization
@Composable
fun OptimizedProductList(products: List<Product>) {
    LazyColumn(
        // Provide stable keys for better performance
        key = { index -> products[index].id }
    ) {
        items(
            items = products,
            key = { product -> product.id } // Stable key
        ) { product ->
            ProductItem(
                product = product,
                onFavoriteClick = remember {
                    { /* Handle favorite */ }
                }
            )
        }
    }
}

@Composable
fun ProductItem(
    product: Product,
    onFavoriteClick: () -> Unit
) {
    // Use remember for expensive operations
    val formattedPrice = remember(product.price) {
        NumberFormat.getCurrencyInstance().format(product.price)
    }
    
    Card(
        modifier = Modifier.fillMaxWidth()
    ) {
        Row {
            AsyncImage(
                model = product.imageUrl,
                contentDescription = null,
                modifier = Modifier.size(80.dp)
            )
            
            Column(modifier = Modifier.weight(1f)) {
                Text(product.name)
                Text(formattedPrice)
            }
            
            IconButton(onClick = onFavoriteClick) {
                Icon(Icons.Default.Favorite, contentDescription = null)
            }
        }
    }
}

// 5. Custom Modifier Optimization
fun Modifier.optimizedClickable(
    onClick: () -> Unit
): Modifier = this.then(
    remember {
        Modifier.clickable { onClick() }
    }
)

// 6. State Reading Optimization
@Composable
fun StateOptimizationExample() {
    val uiState by viewModel.uiState.collectAsState()
    
    // ❌ Bad: Reads all state properties
    Column {
        if (uiState.isLoading) {
            CircularProgressIndicator()
        }
        Text(uiState.title)
        Text(uiState.description)
    }
    
    // ✅ Better: Separate state reading
    StateOptimizedContent(uiState)
}

@Composable
private fun StateOptimizedContent(uiState: UiState) {
    Column {
        LoadingIndicator(isLoading = uiState.isLoading)
        ContentSection(
            title = uiState.title,
            description = uiState.description
        )
    }
}

@Composable
private fun LoadingIndicator(isLoading: Boolean) {
    // Only recomposes when isLoading changes
    if (isLoading) {
        CircularProgressIndicator()
    }
}

@Composable
private fun ContentSection(title: String, description: String) {
    // Only recomposes when title or description changes
    Column {
        Text(title)
        Text(description)
    }
}
```

### **Ngày 4-7: Memory & Performance Best Practices**
```kotlin
// 1. Image Optimization
@Composable
fun OptimizedImageLoading() {
    AsyncImage(
        model = ImageRequest.Builder(LocalContext.current)
            .data("https://example.com/image.jpg")
            .crossfade(true)
            .size(300, 200) // Resize image
            .build(),
        contentDescription = null,
        contentScale = ContentScale.Crop,
        modifier = Modifier
            .size(150.dp, 100.dp)
            .clip(RoundedCornerShape(8.dp))
    )
}

// 2. Lazy Loading Optimization
@Composable
fun OptimizedLazyList() {
    val lazyListState = rememberLazyListState()
    
    LazyColumn(
        state = lazyListState,
        contentPadding = PaddingValues(16.dp),
        verticalArrangement = Arrangement.spacedBy(8.dp),
        // Optimize for better scrolling performance
        flingBehavior = ScrollableDefaults.flingBehavior()
    ) {
        items(
            count = 1000,
            key = { index -> "item_$index" }
        ) { index ->
            LazyListItem(index = index)
        }
    }
    
    // Add scroll to top button for better UX
    val showScrollToTop by remember {
        derivedStateOf {
            lazyListState.firstVisibleItemIndex > 0
        }
    }
    
    if (showScrollToTop) {
        FloatingActionButton(
            onClick = {
                // Use coroutine scope for smooth scrolling
                scope.launch {
                    lazyListState.animateScrollToItem(0)
                }
            }
        ) {
            Icon(Icons.Default.KeyboardArrowUp, contentDescription = "Scroll to top")
        }
    }
}

@Composable
fun LazyListItem(index: Int) {
    // Minimize allocations in list items
    Card(
        modifier = Modifier
            .fillMaxWidth()
            .height(80.dp)
    ) {
        Row(
            modifier = Modifier
                .fillMaxSize()
                .padding(16.dp),
            verticalAlignment = Alignment.CenterVertically
        ) {
            // Use remember for expensive calculations
            val itemTitle = remember(index) { "Item #${index + 1}" }
            val itemSubtitle = remember(index) { "Subtitle for item $index" }
            
            Column {
                Text(
                    text = itemTitle,
                    style = MaterialTheme.typography.titleMedium
                )
                Text(
                    text = itemSubtitle,
                    style = MaterialTheme.typography.bodyMedium
                )
            }
        }
    }
}

// 3. Animation Performance
@Composable
fun PerformantAnimations() {
    var isVisible by remember { mutableStateOf(true) }
    
    // Use AnimatedVisibility instead of manual alpha/scale
    AnimatedVisibility(
        visible = isVisible,
        enter = fadeIn(animationSpec = tween(300)) + 
                scaleIn(animationSpec = tween(300)),
        exit = fadeOut(animationSpec = tween(300)) + 
               scaleOut(animationSpec = tween(300))
    ) {
        ComplexContent()
    }
    
    // For simple animations, use animate*AsState
    val animatedAlpha by animateFloatAsState(
        targetValue = if (isVisible) 1f else 0f,
        animationSpec = tween(300),
        label = "alpha animation"
    )
    
    // Avoid creating new objects in animations
    val animatedColor by animateColorAsState(
        targetValue = if (isVisible) Color.Blue else Color.Gray,
        animationSpec = tween(300),
        label = "color animation"
    )
}

// 4. Custom Layout Performance
@Composable
fun PerformantCustomLayout(
    modifier: Modifier = Modifier,
    content: @Composable () -> Unit
) {
    Layout(
        modifier = modifier,
        content = content
    ) { measurables, constraints ->
        // Cache expensive calculations
        val placeables = measurables.map { measurable ->
            measurable.measure(constraints)
        }
        
        val maxWidth = placeables.maxOfOrNull { it.width } ?: 0
        val totalHeight = placeables.sumOf { it.height }
        
        layout(maxWidth, totalHeight) {
            var yPosition = 0
            placeables.forEach { placeable ->
                placeable.placeRelative(x = 0, y = yPosition)
                yPosition += placeable.height
            }
        }
    }
}

// 5. Memory Leak Prevention
@Composable
fun MemoryLeakPrevention() {
    val context = LocalContext.current
    
    // ❌ Avoid holding references to Context/Activity
    val badViewModel = remember { BadViewModel(context as Activity) }
    
    // ✅ Use Application Context or dependency injection
    val goodViewModel: GoodViewModel = hiltViewModel()
    
    // ✅ Clean up resources properly
    DisposableEffect(Unit) {
        val listener = SomeListener()
        SomeManager.addListener(listener)
        
        onDispose {
            SomeManager.removeListener(listener)
        }
    }
    
    // ✅ Use weak references for callbacks
    val weakCallback = remember {
        WeakReference<() -> Unit> { /* callback */ }
    }
}

// 6. Composition Local Optimization
val LocalUserPreferences = compositionLocalOf<UserPreferences> {
    error("UserPreferences not provided")
}

@Composable
fun OptimizedCompositionLocal() {
    // Provide only what's needed at the right scope
    CompositionLocalProvider(LocalUserPreferences provides userPreferences) {
        // Children can access UserPreferences without prop drilling
        UserInterface()
    }
}

// 7. Performance Monitoring
@Composable
fun PerformanceMonitoring() {
    // Use remember to track recompositions
    val recompositionCount = remember { mutableStateOf(0) }
    
    SideEffect {
        recompositionCount.value++
        if (BuildConfig.DEBUG) {
            Log.d("Recomposition", "Component recomposed ${recompositionCount.value} times")
        }
    }
    
    // Monitor expensive operations
    val expensiveValue = remember {
        Trace.beginSection("ExpensiveCalculation")
        val result = performExpensiveOperation()
        Trace.endSection()
        result
    }
}
```

**Dự án tuần 10: Performance Dashboard**
- Performance metrics monitoring
- Optimized large lists
- Memory-efficient images
- Animation performance tuning

---

Tôi đã tạo xong 2 file tài liệu chi tiết về lộ trình học Jetpack Compose + Android:

## 📁 **Files Created:**

### 1. **Android_Jetpack_Compose_Roadmap.md** 
- Phase 1-2 (Tuần 1-8): Foundation đến Intermediate
- Covers: Kotlin basics, Compose fundamentals, State management, Navigation, Material Design, Side effects

### 2. **Android_Compose_Advanced_Roadmap.md**
- Phase 3-4 (Tuần 9-16): Advanced đến Expert  
- Covers: Animations, Architecture patterns, Testing, Performance optimization

## 🎯 **Roadmap Highlights:**

### **📚 16 Tuần Structured Learning**
- **4 Phases** từ cơ bản đến chuyên gia
- **Daily study plan** 3-4 giờ/ngày
- **Practical projects** mỗi tuần
- **Checkpoint milestones** để track progress

### **💻 Hands-on Code Examples**
- Real-world code samples
- Best practices implementation
- Performance optimization techniques
- Complete app architectures

### **🚀 Production-Ready Skills**
- MVVM architecture với Compose
- Testing strategies (Unit, UI, E2E)
- Performance optimization
- Memory management
- Advanced animations

## 📋 **How to Use:**

1. **Follow sequentially** - Mỗi tuần build trên kiến thức tuần trước
2. **Practice daily** - Code examples và projects
3. **Track progress** - Checkpoints mỗi 4 tuần
4. **Build portfolio** - Projects thực tế để showcase skills

Files này sẽ guide bạn từ beginner hoàn toàn đến có thể làm việc với Jetpack Compose trong production environment! 🚀

Bạn có muốn tôi tạo thêm file nào khác như project templates, practice exercises, hoặc resource links không? 