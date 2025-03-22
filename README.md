# Examen-2
examen 2
using System;
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
using Microsoft.EntityFrameworkCore;
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using System.Linq;

// Modelo para Pacientes
public class Paciente
{
    [Key]
    public int Id { get; set; }

    [Required]
    [StringLength(100)]
    public string Nombre { get; set; }

    [Required]
    [StringLength(100)]
    public string Apellido { get; set; }

    [Required]
    [DataType(DataType.Date)]
    public DateTime FechaNacimiento { get; set; }

    [Required]
    [StringLength(20)]
    public string Telefono { get; set; }
}

// Modelo para Doctores
public class Doctor
{
    [Key]
    public int Id { get; set; }

    [Required]
    [StringLength(100)]
    public string Nombre { get; set; }

    [Required]
    [StringLength(100)]
    public string Especialidad { get; set; }
}

// Modelo para Citas
public class Cita
{
    [Key]
    public int Id { get; set; }

    [Required]
    [ForeignKey("Paciente")]
    public int PacienteId { get; set; }
    public Paciente Paciente { get; set; }

    [Required]
    [ForeignKey("Doctor")]
    public int DoctorId { get; set; }
    public Doctor Doctor { get; set; }

    [Required]
    [DataType(DataType.DateTime)]
    public DateTime FechaCita { get; set; }
}

// Contexto de la Base de Datos
public class ClinicaUHJuanPabloContext : DbContext
{
    public DbSet<Paciente> Pacientes { get; set; }
    public DbSet<Doctor> Doctores { get; set; }
    public DbSet<Cita> Citas { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer("Server=.;Database=CLINICA_UH_JUAN_PABLO;Trusted_Connection=True;");
    }
}

// Controladores Web API
[Route("api/[controller]")]
[ApiController]
public class PacientesController : ControllerBase
{
    private readonly ClinicaUHJuanPabloContext _context;

    public PacientesController(ClinicaUHJuanPabloContext context)
    {
        _context = context;
    }

    [HttpGet]
    public ActionResult<IEnumerable<Paciente>> GetPacientes()
    {
        return _context.Pacientes.ToList();
    }
}

// Frontend con HTML y CSS
public class WebApp
{
    public static string HtmlTemplate = @"<html>
<head>
    <title>Clinica UH Juan Pablo</title>
    <style>
        body { font-family: Arial, sans-serif; }
        .menu { background: #007bff; padding: 10px; }
        .menu a { color: white; margin-right: 10px; text-decoration: none; }
    </style>
</head>
<body>
    <div class='menu'>
        <a href='/pacientes'>Pacientes</a>
        <a href='/doctores'>Doctores</a>
        <a href='/citas'>Citas</a>
    </div>
    <h1>Bienvenido a Clinica UH Juan Pablo</h1>
</body>
</html>";
}
