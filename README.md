# Comando para crear .gz

## Prueba 1
`estar en la raiz`
```bash
# 2. Crear build.sh en la raíz
cat > build.sh << 'EOF'
#!/bin/bash
echo "🔨 Building Talos CLI..."

# Ir al proyecto
cd Talos.Tool

# Limpiar
rm -rf bin/ obj/ publish/

# Construir
dotnet publish -c Release -r linux-x64 \
    --self-contained true \
    -p:PublishSingleFile=true \
    -p:PublishTrimmed=false \
    -p:AssemblyName=talos \
    -o ./publish/linux-x64

# Dar permisos
chmod +x ./publish/linux-x64/talos

echo "✅ Build completado!"
echo "📦 Ejecutable: Talos.Tool/publish/linux-x64/talos"
EOF

chmod +x build.sh

# 3. Ejecutar build
./build.sh

# 4. Verificar que se creó
ls -la Talos.Tool/publish/linux-x64/talos

# 5. Probar
Talos.Tool/publish/linux-x64/talos --help

```
## Crear Paquete para el repo: 
```bash
# Crear paquete en la raíz
mkdir -p talos-package
cp Talos.Tool/publish/linux-x64/talos talos-package/
tar -czf talos-linux-x64.tar.gz talos-package/

echo "📦 Paquete creado: talos-linux-x64.tar.gz"
```

## instalar con sudo:
```bash
# Instalar en /usr/local/bin (recomendado)
sudo cp Talos.Tool/publish/linux-x64/talos /usr/local/bin/
talos hello --name "Global"

# Verificar instalación
which talos
talos --help
```
