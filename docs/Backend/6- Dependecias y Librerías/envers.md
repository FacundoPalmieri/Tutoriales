---
sidebar_position: 2
---

# 2 - Hibernate Envers


## Tablas de audítoria - Hibernate Envers (@Audited)

@Audited es una anotación que ofrece Hibernate Envers para registrar automáticamente los cambios históricos de una entidad en la base de datos. Cada vez que se crea, actualiza o elimina una entidad, se guarda una copia del estado anterior en una tabla de auditoría.

Así, podés consultar el "historial" de una entidad, como si fuera un sistema de versiones.

### Requisitos previos

-   Tener una app Spring Boot con JPA y una base de datos configurada (H2, PostgreSQL, MySQL, etc.)

-   Usar Hibernate como proveedor JPA (es el default en Spring Boot)

### Paso 1: Agregar la dependencia de Hibernate Envers
```jsx title="POM.xml"
	<dependency>
			<groupId>org.hibernate.orm</groupId>
			<artifactId>hibernate-core</artifactId>
			<version>6.4.4.Final</version>
		</dependency>


		<!--Dependencia para generar tablas de auditoría ante cambios de persistencia -->
		<dependency>
			<groupId>org.hibernate.orm</groupId>
			<artifactId>hibernate-envers</artifactId>
			<version>6.4.4.Final</version>
		</dependency>

``` 

### Paso 2: Anotar las entidades con @Audited

En la entidad a auditar debemos:

-   Colocar @Audited como anotación de la entidad

En caso que querramos excluir algún atributo podremos hacerlo de la siguiente manera:

-    @NotAudited : Para campo simples

-   @Audited(targetAuditMode = RelationTargetAuditMode.NOT_AUDITED) : Para campos donde existan relaciones (Ej: @ManyToOne)

En el caso que un campo con una relación deba ser auditado, la **entidad relacionada** también deberá llevar la anotación  @Audited

```jsx title=""
@Entity
@Inheritance(strategy = InheritanceType.JOINED)
@Data
@Audited <----
public abstract class Person {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(length = 30, nullable = false)
    private String firstName;

    @Column(length = 30, nullable = false)
    @NotAudited  <-----
    private String lastName;

    @ManyToOne(fetch = FetchType.LAZY, targetEntity = DniType.class)
    @Audited(targetAuditMode = RelationTargetAuditMode.NOT_AUDITED) <-----
    @JoinColumn(name = "dni_type_id")
    private DniType dniType;

}
``` 



