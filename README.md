# TP3_-61136
using System;
using System.Collections;
using System.Collections.Generic;

// ===================
// Clase genérica ListaOrdenada<T>
// ===================
public class ListaOrdenada<T> : IEnumerable<T> where T : IComparable<T>
{
    private List<T> elementos = new List<T>();

    public int Cantidad => elementos.Count;

    public T this[int indice] => elementos[indice];

    public bool Contiene(T elemento)
    {
        return elementos.Contains(elemento);
    }

    public void Agregar(T elemento)
    {
        if (Contiene(elemento)) return;

        int index = elementos.BinarySearch(elemento);
        if (index < 0) index = ~index;
        elementos.Insert(index, elemento);
    }

    public void Eliminar(T elemento)
    {
        elementos.Remove(elemento);
    }

    public ListaOrdenada<T> Filtrar(Predicate<T> condicion)
    {
        var nuevaLista = new ListaOrdenada<T>();
        foreach (var elemento in elementos)
        {
            if (condicion(elemento))
            {
                nuevaLista.Agregar(elemento);
            }
        }
        return nuevaLista;
    }

    public IEnumerator<T> GetEnumerator()
    {
        return elementos.GetEnumerator();
    }

    IEnumerator IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }
}

// ===================
// Clase Contacto
// ===================
public class Contacto : IComparable<Contacto>
{
    public string Nombre { get; set; }
    public string Telefono { get; set; }

    public Contacto(string nombre, string telefono)
    {
        Nombre = nombre;
        Telefono = telefono;
    }

    public int CompareTo(Contacto otro)
    {
        return string.Compare(this.Nombre, otro.Nombre, StringComparison.OrdinalIgnoreCase);
    }

    public override bool Equals(object obj)
    {
        if (obj is Contacto c)
            return this.Nombre.Equals(c.Nombre, StringComparison.OrdinalIgnoreCase);
        return false;
    }

    public override int GetHashCode()
    {
        return Nombre.ToLower().GetHashCode();
    }

    public override string ToString()
    {
        return $"{Nombre} - {Telefono}";
    }
}

// ===================
// Programa principal
// ===================
class Program
{
    static void Main()
    {
        Console.WriteLine("== ListaOrdenada<int> ==");
        var numeros = new ListaOrdenada<int>();
        numeros.Agregar(10);
        numeros.Agregar(5);
        numeros.Agregar(20);
        numeros.Agregar(10); // Duplicado, ignorado

        Console.WriteLine("Números ordenados:");
        foreach (var n in numeros)
            Console.WriteLine(n);

        Console.WriteLine($"¿Contiene 5?: {numeros.Contiene(5)}");
        numeros.Eliminar(5);
        Console.WriteLine("Después de eliminar 5:");
        foreach (var n in numeros)
            Console.WriteLine(n);

        Console.WriteLine($"\nElemento en índice 0: {numeros[0]}");
        Console.WriteLine($"Cantidad total: {numeros.Cantidad}");

        Console.WriteLine("\n== ListaOrdenada<string> ==");
        var palabras = new ListaOrdenada<string>();
        palabras.Agregar("pera");
        palabras.Agregar("manzana");
        palabras.Agregar("banana");

        Console.WriteLine("Palabras ordenadas:");
        foreach (var p in palabras)
            Console.WriteLine(p);

        var filtradas = palabras.Filtrar(p => p.Contains("a"));
        Console.WriteLine("Palabras que contienen 'a':");
        foreach (var f in filtradas)
            Console.WriteLine(f);

        Console.WriteLine("\n== ListaOrdenada<Contacto> ==");
        var contactos = new ListaOrdenada<Contacto>();
        contactos.Agregar(new Contacto("Carlos", "123"));
        contactos.Agregar(new Contacto("Ana", "456"));
        contactos.Agregar(new Contacto("Luis", "789"));
        contactos.Agregar(new Contacto("Ana", "000")); // Duplicado por nombre, se ignora

        Console.WriteLine("Contactos ordenados:");
        foreach (var c in contactos)
            Console.WriteLine(c);

        Console.WriteLine("\nFiltrar contactos que comienzan con 'C':");
        var contactosFiltrados = contactos.Filtrar(c => c.Nombre.StartsWith("C"));
        foreach (var c in contactosFiltrados)
            Console.WriteLine(c);

        Console.WriteLine($"\nCantidad total de contactos: {contactos.Cantidad}");
    }
}
