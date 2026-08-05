@startuml Diagrama_Casos_de_Uso_Registro_BarberIA

left to right direction
skinparam packageStyle rectangle
skinparam shadowing false
skinparam actorStyle stickman

' ==========================================
' ACTORES
' ==========================================
actor "Usuario" as user
actor "Cliente" as client
actor "Barbero" as barber
actor "Administrador" as admin

actor "Sistema" as sys
database "Base de Datos" as bd

' Herencia de Actores (Todos son Usuarios en el sistema)
user <|-- client
user <|-- barber
user <|-- admin

' ==========================================
' SISTEMA DE GESTION DE REGISTRO Y ACCESOS
' ==========================================
rectangle "Sistema de Gestión de Registro" {

' --- Bloque 1: Registro de Usuarios ---
usecase "Ingresar Nuevo Registro\n(tipo_documento, correo, password, rol)" as UC_NewRegister
usecase "Asignar Datos Automáticos\n(usuario_id, fecha_registro, estado)" as UC_AutoAssign
usecase "Guardar Registro de Usuario +\nGeneración de IDs (INSERT)" as UC_InsertUserRecord

' --- Bloque 2: Gestión de Perfil y Datos ---
usecase "Ingresar al Perfil" as UC_AccessProfile
usecase "Ingresar Datos al Perfil" as UC_InputProfileData
usecase "Insertar Datos (INSERT)" as UC_InsertData

usecase "Modificar Datos del Perfil" as UC_EditProfileData
usecase "Actualizar Datos (UPDATE)" as UC_UpdateData

usecase "Eliminar Datos del Perfil" as UC_DeleteProfileData
usecase "Eliminar Datos / Soft Delete\n(DELETE/UPDATE)" as UC_SoftDeleteData

usecase "Consultar Datos de Perfil\n(SELECT)" as UC_SelectData


}

' ==========================================
' CONEXIONES - ACTOR USUARIO (Humano)
' ==========================================
user --> UC_AccessProfile
user --> UC_NewRegister

' ==========================================
' CONEXIONES - ACTOR SISTEMA (Procesos Automáticos)
' ==========================================
sys --> UC_AccessProfile
sys --> UC_AutoAssign

' ==========================================
' RELACIONES DE INCLUSIÓN Y EXTENSIÓN (Plantilla Exacta)
' ==========================================
' Relaciones del Registro Nuevo
UC_AutoAssign ..> UC_InsertUserRecord : <>
UC_AutoAssign <.. UC_NewRegister : <>

' Relaciones del Perfil de Usuario
UC_AccessProfile ..> UC_InputProfileData : <>
UC_AccessProfile ..> UC_EditProfileData : <>
UC_AccessProfile ..> UC_DeleteProfileData : <>
UC_AccessProfile ..> UC_SelectData : <>

' Inclusiones hacia las Operaciones SQL en BD
UC_InputProfileData ..> UC_InsertData : <>
UC_EditProfileData ..> UC_UpdateData : <>
UC_DeleteProfileData ..> UC_SoftDeleteData : <>

' ==========================================
' CONEXIONES - BASE DE DATOS (Persistencia)
' ==========================================
UC_NewRegister --> bd
UC_InsertUserRecord --> bd
UC_InsertData --> bd
UC_UpdateData --> bd
UC_SoftDeleteData --> bd
UC_SelectData --> bd

@enduml
