# 💻 Acesso a Dados com JDBC (Java Database Connectivity)

Este projeto tem como objetivo apresentar, na prática, os principais recursos do **JDBC** — a API padrão do Java para acesso a bancos de dados — e a implementação manual do **padrão DAO (Data Access Object)**.

Material baseado no curso **“Programação Orientada a Objetos com Java”**, do Prof. Dr. Nelio Alves (Educando Web).

---

## 🎯 Objetivos do Projeto

- Conhecer os principais recursos do **JDBC** na teoria e na prática.  
- Elaborar a **estrutura básica** de um projeto Java conectado a banco de dados.  
- Implementar o **padrão DAO** manualmente utilizando JDBC.  

---

## 🧠 Visão Geral

**JDBC (Java Database Connectivity)** é a API padrão do Java para comunicação com bancos de dados relacionais.

📚 Documentação oficial:
- [Guia JDBC - Oracle](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/)
- [Pacote java.sql](https://docs.oracle.com/javase/8/docs/api/java/sql/package-summary.html)

Principais pacotes utilizados:
```java
import java.sql.*;
import javax.sql.*;
```

---

## ⚙️ Instalação e Preparação do Ambiente

### 🧩 Requisitos
- **Java JDK 8+**
- **Eclipse IDE**
- **MySQL Server** e **MySQL Workbench**
- **MySQL Connector/J** (driver JDBC)

### 🔧 Passos para Configuração

1. **Criar a base de dados:**
   ```sql
   CREATE DATABASE coursejdbc;
   ```
2. **Criar o arquivo de propriedades (`db.properties`)** na raiz do projeto:
   ```properties
   user=developer
   password=1234567
   dburl=jdbc:mysql://localhost:3306/coursejdbc
   useSSL=false
   ```
3. **Adicionar o MySQL Connector ao projeto:**
   - Vá em `Window → Preferences → Java → Build Path → User Libraries`
   - Crie uma nova biblioteca chamada `MySQLConnector`
   - Adicione o arquivo `.jar` do conector
   - Inclua essa biblioteca no projeto Java

---

## 🗂️ Estrutura do Projeto

```
src/
 ├── db/
 │   ├── DB.java
 │   ├── DbException.java
 │   └── DbIntegrityException.java
 ├── model/
 │   ├── entities/
 │   │   ├── Department.java
 │   │   └── Seller.java
 │   ├── dao/
 │   │   ├── DepartmentDao.java
 │   │   ├── SellerDao.java
 │   │   └── impl/
 │   │       ├── DepartmentDaoJDBC.java
 │   │       └── SellerDaoJDBC.java
 │   └── DaoFactory.java
 └── application/
     ├── Program.java
     └── Program2.java
```

---

## 🧩 Funcionalidades Demonstradas

### 1️⃣ **Recuperação de Dados**
- Uso de `Statement` e `ResultSet`
- Navegação:
  ```java
  rs.next();
  rs.first();
  rs.beforeFirst();
  rs.absolute(int position);
  ```

### 2️⃣ **Inserção de Dados**
- Uso de `PreparedStatement`
- Recuperação de chaves geradas automaticamente:
  ```java
  Statement.RETURN_GENERATED_KEYS;
  getGeneratedKeys();
  ```

### 3️⃣ **Atualização e Exclusão**
- Uso de `executeUpdate()`
- Tratamento de integridade referencial com `DbIntegrityException`

### 4️⃣ **Transações**
- Controle manual de commit/rollback:
  ```java
  conn.setAutoCommit(false);
  conn.commit();
  conn.rollback();
  ```

---

## 🏗️ Padrão DAO (Data Access Object)

### Conceito
Cada entidade do sistema (ex: `Seller`, `Department`) possui um objeto responsável pelo acesso aos dados — o DAO correspondente.

### Estrutura
- Interface DAO para cada entidade (`SellerDao`, `DepartmentDao`)
- Implementações JDBC (`SellerDaoJDBC`, `DepartmentDaoJDBC`)
- Instanciação via **Factory Pattern** (`DaoFactory`)

### Exemplo de Query (findById)
```sql
SELECT seller.*, department.Name AS DepName
FROM seller
INNER JOIN department
ON seller.DepartmentId = department.Id
WHERE seller.Id = ?
```

---

## 🧠 Reuso de Instanciação

```java
private Seller instantiateSeller(ResultSet rs, Department dep) throws SQLException {
    Seller obj = new Seller();
    obj.setId(rs.getInt("Id"));
    obj.setName(rs.getString("Name"));
    obj.setEmail(rs.getString("Email"));
    obj.setBaseSalary(rs.getDouble("BaseSalary"));
    obj.setBirthDate(rs.getDate("BirthDate"));
    obj.setDepartment(dep);
    return obj;
}

private Department instantiateDepartment(ResultSet rs) throws SQLException {
    Department dep = new Department();
    dep.setId(rs.getInt("DepartmentId"));
    dep.setName(rs.getString("DepName"));
    return dep;
}
```

---

## 🔌 Integração com GitHub

### Criar o repositório
```bash
git init
git remote add origin https://github.com/seuusuario/jdbc-dao-demo.git
git add .
git commit -m "Projeto JDBC criado"
git push -u origin master
```

> 💡 Use `.gitignore` do tipo **Java** para evitar subir arquivos desnecessários.

---

## 🧾 Referências

- Curso “Programação Orientada a Objetos com Java” – Prof. Nelio Alves  
- [DAO Pattern - DevMedia](https://www.devmedia.com.br/dao-pattern-persistencia-de-dados-utilizando-o-padrao-dao/30999)  
- [DAO Pattern - Oracle](https://www.oracle.com/technetwork/java/dataaccessobject-138824.html)  
- [Exemplo de código oficial](https://github.com/acenelio/demo-dao-jdbc)

---

## 🧑‍💻 Autor

**Rafael Kmohan Paulino Patricio**  
📘 Projeto educacional desenvolvido com base no material do Prof. Nelio Alves.  
🔗 GitHub: [github.com/r121990](https://github.com/r121990)

---
