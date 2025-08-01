<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "50d9cd44fa74ad04f716fe31daf0c850",
  "translation_date": "2025-06-13T00:32:42+00:00",
  "source_file": "05-AdvancedTopics/mcp-security/README.md",
  "language_code": "id"
}
-->
# Praktik Terbaik Keamanan

Keamanan sangat penting untuk implementasi MCP, terutama di lingkungan perusahaan. Penting untuk memastikan bahwa alat dan data terlindungi dari akses tidak sah, kebocoran data, dan ancaman keamanan lainnya.

## Pendahuluan

Dalam pelajaran ini, kita akan membahas praktik terbaik keamanan untuk implementasi MCP. Kita akan membahas otentikasi dan otorisasi, perlindungan data, eksekusi alat yang aman, serta kepatuhan terhadap peraturan privasi data.

## Tujuan Pembelajaran

Pada akhir pelajaran ini, Anda akan mampu:

- Menerapkan mekanisme otentikasi dan otorisasi yang aman untuk server MCP.
- Melindungi data sensitif menggunakan enkripsi dan penyimpanan yang aman.
- Memastikan eksekusi alat yang aman dengan kontrol akses yang tepat.
- Menerapkan praktik terbaik untuk perlindungan data dan kepatuhan privasi.

## Otentikasi dan Otorisasi

Otentikasi dan otorisasi sangat penting untuk mengamankan server MCP. Otentikasi menjawab pertanyaan "Siapa Anda?" sementara otorisasi menjawab "Apa yang bisa Anda lakukan?".

Mari kita lihat contoh bagaimana menerapkan otentikasi dan otorisasi yang aman pada server MCP menggunakan .NET dan Java.

### Integrasi Identitas .NET

ASP .NET Core Identity menyediakan kerangka kerja yang kuat untuk mengelola otentikasi dan otorisasi pengguna. Kita dapat mengintegrasikannya dengan server MCP untuk mengamankan akses ke alat dan sumber daya.

Ada beberapa konsep inti yang perlu kita pahami saat mengintegrasikan ASP.NET Core Identity dengan server MCP, yaitu:

- **Konfigurasi Identity**: Menyiapkan ASP.NET Core Identity dengan peran dan klaim pengguna. Klaim adalah informasi tentang pengguna, seperti peran atau izin mereka, misalnya "Admin" atau "User".
- **Otentikasi JWT**: Menggunakan JSON Web Tokens (JWT) untuk akses API yang aman. JWT adalah standar untuk mengirim informasi secara aman antar pihak sebagai objek JSON, yang dapat diverifikasi dan dipercaya karena ditandatangani secara digital.
- **Kebijakan Otorisasi**: Mendefinisikan kebijakan untuk mengontrol akses ke alat tertentu berdasarkan peran pengguna. MCP menggunakan kebijakan otorisasi untuk menentukan pengguna mana yang dapat mengakses alat berdasarkan peran dan klaim mereka.

```csharp
public class SecureMcpStartup
{
    public void ConfigureServices(IServiceCollection services)
    {
        // Add ASP.NET Core Identity
        services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
        
        // Configure JWT authentication
        services.AddAuthentication(options =>
        {
            options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
            options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
        })
        .AddJwtBearer(options =>
        {
            options.TokenValidationParameters = new TokenValidationParameters
            {
                ValidateIssuer = true,
                ValidateAudience = true,
                ValidateLifetime = true,
                ValidateIssuerSigningKey = true,
                ValidIssuer = Configuration["Jwt:Issuer"],
                ValidAudience = Configuration["Jwt:Audience"],
                IssuerSigningKey = new SymmetricSecurityKey(
                    Encoding.UTF8.GetBytes(Configuration["Jwt:Key"]))
            };
        });
        
        // Add authorization policies
        services.AddAuthorization(options =>
        {
            options.AddPolicy("CanUseAdminTools", policy =>
                policy.RequireRole("Admin"));
                
            options.AddPolicy("CanUseBasicTools", policy =>
                policy.RequireAuthenticatedUser());
        });
        
        // Configure MCP server with security
        services.AddMcpServer(options =>
        {
            options.ServerName = "Secure MCP Server";
            options.ServerVersion = "1.0.0";
            options.RequireAuthentication = true;
        });
        
        // Register tools with authorization requirements
        services.AddMcpTool<BasicTool>(options => 
            options.RequirePolicy("CanUseBasicTools"));
            
        services.AddMcpTool<AdminTool>(options => 
            options.RequirePolicy("CanUseAdminTools"));
    }
    
    public void Configure(IApplicationBuilder app)
    {
        // Use authentication and authorization
        app.UseAuthentication();
        app.UseAuthorization();
        
        // Use MCP server middleware
        app.UseMcpServer();
    }
}
```

Dalam kode di atas, kita telah:

- Mengonfigurasi ASP.NET Core Identity untuk manajemen pengguna.
- Menyiapkan otentikasi JWT untuk akses API yang aman. Kita menentukan parameter validasi token, termasuk issuer, audience, dan signing key.
- Mendefinisikan kebijakan otorisasi untuk mengontrol akses ke alat berdasarkan peran pengguna. Misalnya, kebijakan "CanUseAdminTools" mengharuskan pengguna memiliki peran "Admin", sedangkan kebijakan "CanUseBasic" mengharuskan pengguna sudah terotentikasi.
- Mendaftarkan alat MCP dengan persyaratan otorisasi spesifik, memastikan hanya pengguna dengan peran yang sesuai yang dapat mengaksesnya.

### Integrasi Java Spring Security

Untuk Java, kita akan menggunakan Spring Security untuk menerapkan otentikasi dan otorisasi yang aman pada server MCP. Spring Security menyediakan kerangka kerja keamanan yang komprehensif dan terintegrasi mulus dengan aplikasi Spring.

Konsep inti di sini adalah:

- **Konfigurasi Spring Security**: Menyiapkan konfigurasi keamanan untuk otentikasi dan otorisasi.
- **OAuth2 Resource Server**: Menggunakan OAuth2 untuk akses aman ke alat MCP. OAuth2 adalah kerangka kerja otorisasi yang memungkinkan layanan pihak ketiga menukar token akses untuk akses API yang aman.
- **Interceptor Keamanan**: Menerapkan interceptor keamanan untuk menegakkan kontrol akses pada eksekusi alat.
- **Kontrol Akses Berbasis Peran**: Menggunakan peran untuk mengontrol akses ke alat dan sumber daya tertentu.
- **Anotasi Keamanan**: Menggunakan anotasi untuk mengamankan metode dan endpoint.

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .authorizeRequests()
                .antMatchers("/mcp/discovery").permitAll() // Allow tool discovery
                .antMatchers("/mcp/tools/**").hasAnyRole("USER", "ADMIN") // Require authentication for tools
                .antMatchers("/mcp/admin/**").hasRole("ADMIN") // Admin-only endpoints
                .anyRequest().authenticated()
            .and()
            .oauth2ResourceServer().jwt();
    }
    
    @Bean
    public McpSecurityInterceptor mcpSecurityInterceptor() {
        return new McpSecurityInterceptor();
    }
}

// MCP Security Interceptor for tool authorization
public class McpSecurityInterceptor implements ToolExecutionInterceptor {
    @Autowired
    private JwtDecoder jwtDecoder;
    
    @Override
    public void beforeToolExecution(ToolRequest request, Authentication authentication) {
        String toolName = request.getToolName();
        
        // Check if user has permissions for this tool
        if (toolName.startsWith("admin") && !authentication.getAuthorities().contains("ROLE_ADMIN")) {
            throw new AccessDeniedException("You don't have permission to use this tool");
        }
        
        // Additional security checks based on tool or parameters
        if ("sensitiveDataAccess".equals(toolName)) {
            validateDataAccessPermissions(request, authentication);
        }
    }
    
    private void validateDataAccessPermissions(ToolRequest request, Authentication auth) {
        // Implementation to check fine-grained data access permissions
    }
}
```

Dalam kode di atas, kita telah:

- Mengonfigurasi Spring Security untuk mengamankan endpoint MCP, memungkinkan akses publik untuk penemuan alat sambil mengharuskan otentikasi untuk eksekusi alat.
- Menggunakan OAuth2 sebagai resource server untuk menangani akses aman ke alat MCP.
- Menerapkan interceptor keamanan untuk menegakkan kontrol akses pada eksekusi alat, memeriksa peran dan izin pengguna sebelum mengizinkan akses ke alat tertentu.
- Mendefinisikan kontrol akses berbasis peran untuk membatasi akses ke alat admin dan akses data sensitif berdasarkan peran pengguna.

## Perlindungan Data dan Privasi

Perlindungan data sangat penting untuk memastikan bahwa informasi sensitif ditangani dengan aman. Ini termasuk melindungi informasi pribadi yang dapat diidentifikasi (PII), data keuangan, dan informasi sensitif lainnya dari akses tidak sah dan kebocoran.

### Contoh Perlindungan Data Python

Mari kita lihat contoh bagaimana menerapkan perlindungan data di Python menggunakan enkripsi dan deteksi PII.

```python
from mcp_server import McpServer
from mcp_tools import Tool, ToolRequest, ToolResponse
from cryptography.fernet import Fernet
import os
import json
from functools import wraps

# PII Detector - identifies and protects sensitive information
class PiiDetector:
    def __init__(self):
        # Load patterns for different types of PII
        with open("pii_patterns.json", "r") as f:
            self.patterns = json.load(f)
    
    def scan_text(self, text):
        """Scans text for PII and returns detected PII types"""
        detected_pii = []
        # Implementation to detect PII using regex or ML models
        return detected_pii
    
    def scan_parameters(self, parameters):
        """Scans request parameters for PII"""
        detected_pii = []
        for key, value in parameters.items():
            if isinstance(value, str):
                pii_in_value = self.scan_text(value)
                if pii_in_value:
                    detected_pii.append((key, pii_in_value))
        return detected_pii

# Encryption Service for protecting sensitive data
class EncryptionService:
    def __init__(self, key_path=None):
        if key_path and os.path.exists(key_path):
            with open(key_path, "rb") as key_file:
                self.key = key_file.read()
        else:
            self.key = Fernet.generate_key()
            if key_path:
                with open(key_path, "wb") as key_file:
                    key_file.write(self.key)
        
        self.cipher = Fernet(self.key)
    
    def encrypt(self, data):
        """Encrypt data"""
        if isinstance(data, str):
            return self.cipher.encrypt(data.encode()).decode()
        else:
            return self.cipher.encrypt(json.dumps(data).encode()).decode()
    
    def decrypt(self, encrypted_data):
        """Decrypt data"""
        if encrypted_data is None:
            return None
        
        decrypted = self.cipher.decrypt(encrypted_data.encode())
        try:
            return json.loads(decrypted)
        except:
            return decrypted.decode()

# Security decorator for tools
def secure_tool(requires_encryption=False, log_access=True):
    def decorator(cls):
        original_execute = cls.execute_async if hasattr(cls, 'execute_async') else cls.execute
        
        @wraps(original_execute)
        async def secure_execute(self, request):
            # Check for PII in request
            pii_detector = PiiDetector()
            pii_found = pii_detector.scan_parameters(request.parameters)
            
            # Log access if required
            if log_access:
                tool_name = self.get_name()
                user_id = request.context.get("user_id", "anonymous")
                log_entry = {
                    "timestamp": datetime.now().isoformat(),
                    "tool": tool_name,
                    "user": user_id,
                    "contains_pii": bool(pii_found),
                    "parameters": {k: "***" for k in request.parameters.keys()}  # Don't log actual values
                }
                logging.info(f"Tool access: {json.dumps(log_entry)}")
            
            # Handle detected PII
            if pii_found:
                # Either encrypt sensitive data or reject the request
                if requires_encryption:
                    encryption_service = EncryptionService("keys/tool_key.key")
                    for param_name, pii_types in pii_found:
                        # Encrypt the sensitive parameter
                        request.parameters[param_name] = encryption_service.encrypt(
                            request.parameters[param_name]
                        )
                else:
                    # If encryption not available but PII found, you might reject the request
                    raise ToolExecutionException(
                        "Request contains sensitive data that cannot be processed securely"
                    )
            
            # Execute the original method
            return await original_execute(self, request)
        
        # Replace the execute method
        if hasattr(cls, 'execute_async'):
            cls.execute_async = secure_execute
        else:
            cls.execute = secure_execute
        return cls
    
    return decorator

# Example of a secure tool with the decorator
@secure_tool(requires_encryption=True, log_access=True)
class SecureCustomerDataTool(Tool):
    def get_name(self):
        return "customerData"
    
    def get_description(self):
        return "Accesses customer data securely"
    
    def get_schema(self):
        # Schema definition
        return {}
    
    async def execute_async(self, request):
        # Implementation would access customer data securely
        # Since we used the decorator, PII is already detected and encrypted
        return ToolResponse(result={"status": "success"})
```

Dalam kode di atas, kita telah:

- Menerapkan `PiiDetector` class to scan text and parameters for personally identifiable information (PII).
- Created an `EncryptionService` class to handle encryption and decryption of sensitive data using the `cryptography` library.
- Defined a `secure_tool` decorator that wraps tool execution to check for PII, log access, and encrypt sensitive data if required.
- Applied the `secure_tool` decorator to a sample tool (`SecureCustomerDataTool`) untuk memastikan alat tersebut menangani data sensitif dengan aman.

## Selanjutnya

- [5.9 Web search](../web-search-mcp/README.md)

**Penafian**:  
Dokumen ini telah diterjemahkan menggunakan layanan terjemahan AI [Co-op Translator](https://github.com/Azure/co-op-translator). Meskipun kami berupaya untuk akurasi, harap diketahui bahwa terjemahan otomatis mungkin mengandung kesalahan atau ketidakakuratan. Dokumen asli dalam bahasa aslinya harus dianggap sebagai sumber yang sah. Untuk informasi penting, disarankan menggunakan terjemahan profesional oleh manusia. Kami tidak bertanggung jawab atas kesalahpahaman atau salah tafsir yang timbul dari penggunaan terjemahan ini.