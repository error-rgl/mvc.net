# mvc.net

....

Emergency
https://github.com/CodigoEstudiante/079_ProyectoNCapasCore/tree/master/ProyectoCrud.AplicacionWeb

para relaciones:
https://www.youtube.com/watch?v=QVRaYIioikE

origin:
https://www.youtube.com/watch?v=W_aB-Rr8vhI&t=0s
............

en capa data > Repository > CategoryRepository:

Metodo actualizar:
_bdAldoContext.Categories.Update(modelo);
await _bdAldoContext.SaveChangesAsync();
return true;

Eliminar:
Category modelo = _bdAldoContext.Categories.First(c => c.CategoryId == id);
_bdAldoContext.Categories.Remove(modelo);
await _bdAldoContext.SaveChangesAsync();
return true;

Insertar:
_bdAldoContext.Categories.Add(modelo);
await _bdAldoContext.SaveChangesAsync();
return true;

Obtener: 
return await _bdAldoContext.Categories.FindAsync(id);


ObtenerTodo:
IQueryable<Category> queryCategorySQL = _bdAldoContext.Categories;
return queryCategorySQL;

--------
en capa controller
carpeta / Service
luego de la hoja ... al presionar el foquito continua el contenido de las interfaces...

private readonly IGenericRepository<Category> _categotyRepository;
// Metodo
public CategoryService(IGenericRepository<Category> categoryRepo)
{
    _categotyRepository = categoryRepo;
}
public async Task<bool> Actualizar(Category modelo)
{
    return await _categotyRepository.Actualizar(modelo);
}

public async Task<bool> Eliminar(int id)
{
    return await _categotyRepository.Eliminar(id);
}

public async Task<bool> Insertar(Category modelo)
{
    return await _categotyRepository.Insertar(modelo);
}

public async Task<Category> Obtener(int id)
{
    return await _categotyRepository.Obtener(id);
}

public async Task<IQueryable<Category>> ObtenerTodo()
{
    return await _categotyRepository.ObtenerTodo();
}

Aqui iria la logica del join/ claro tiene que estar en ICategoryService: como: Task<Category> ObtenerNombreJoin(string name)


---------------
en capa appweb > En Program.cs (line 10 aprox.)

builder.Services.AddDbContext<BdAldoContext>(opciones =>
{
    opciones.UseSqlServer(builder.Configuration.GetConnectionString("cadenaSQL"));
});

builder.Services.AddScoped<IGenericRepository<Category>, CategoryRepository>();
builder.Services.AddScoped<ICategoryService, CategoryService>();

---
capa app:  en homeController
la linea 9 a 11 o 12 cambiamos por 

private readonly ICategoryService _categoryService;
public HomeController(ICategoryService categoryServ)
{
    _categoryService = categoryServ;
}

en la misma capa / en la carpeta Models > agregamos carpeta ViewModels > agregar clase name= VMCategory (aqui copiamos la spropiedades de mi capa models de category)

ver guia git...

luego a la carpeta views , archivo index (borramos el por defecto)
