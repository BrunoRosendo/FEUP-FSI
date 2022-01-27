# Trabalho realizado na Semana #4

## Task 1 : Manipulating Environment Variables
*printenv* and *env* can be used to print the environment variables of a system. 

If you want to search for a specific environment variable you can use:

> `printenv <env_key>` or `env | grep <env_key>` 

To add / remove environment variables, you can use *export* and *unset*, respectively.

`export <env_key>=<env_value>` and `unset <env_key>`

---

## Task 2 : Passing Environment Variables from Parent Process to Child Process

### Step 1
The child process prints all its environment variables

### Step 2
The parent process also prints all its environment variables

### Step 3
The child process inherits all the environment variables from the parent, since the files containing the environment variables are equal.

---
## Task 3 : Environment Variables and execve()
### Step 1
The executed program doesn't print anything, so we can conclude it does not inherit any environment variable.

### Step 2
After including the environment variables in the function call *execve*, the new program prints all the environment variables.

### Step 3
The new program receives its environment variables through the function call, *execve*, third argument, that expects a string  buffer. 

---
## Task 4 : Environment Variables and system()
In fact, using the *system* function, it prints all the environment variables, without needing to specify them in the function call.

---
## Task 5 : Environment Variable and Set-UID Programs
### Step 3
While the *PATH* and *ANY_NAME* environment variables get into the child process, the *LD_LIBRARY_PATH* variable is lost. This missing variable was a surprise to us.

---
## Task 6 : The PATH Environment Variable and Set-UID Programs
We were able to run our own malicous code, by manipulating the PATH environment variable and pre-append the path where we inserted our malicious code (the executable named 'ls'). 

We concluded that when we provide a relative path to the *system* function, the system searches in the PATH env. variable for an executable that matches the given relative path, starting from the first string to the last.

However, the malicious code is not running with the root privileges, for the reasons mentioned in the note below.

---
## Task 7 : The LD PRELOAD Environment Variable and Set-UID Programs

### Step 3
1. Regular Program
   - Runs the malicious *sleep* executable.
2. SET-UID root Program, while running as normal user
   - Runs the system *sleep* (LD_PRELOAD env variable is not inherited)
3. SET-UID root Program, export env. variable in the root and run
   - Runs the malicious code
4. SET-UID user1 Program, export .env variable in the user1 and run it
   - Runs the system *sleep*


## CTF - Desafio 1

O desafio 1 consiste em descobrir a vulnerabilidade presente no website que nos é disponibilizado. Pesquisando por vulnerabilidades relacionadas com WordPress e com a versão 5.4.3 do plugin WooCommerce Booster, em sites como exploit-db, cvedetails e cve.gist. Com isto, e com algum processo de tentativa e erro, descobrimos que a vulnerabilidade alvo era a CVE-2021-34646, cujo nome foi usado como flag ( flag{CVE-2021-34646} )

A CVE pode ser encontrada aqui: https://nvd.nist.gov/vuln/detail/CVE-2021-34646


## CTF - Desafio 2

O desafio 2 consistiu em descobrir e utilizar um exploit que tirasse partido da CVE encontrada no desafio 1. Assim sendo, com alguma pesquisa foi encontrado o seguinte exploit: https://www.exploit-db.com/exploits/50299.
Usamos, então, a script python fornecida no exploit no website fornecido na CTF. O processo foi o seguinte (tal como descrito nos comentários da script):
- Ir ao seguinte endereço web, de forma a retirar o ID do user a qual queríamos aceder: http://ctf-fsi.fe.up.pt:5001/wp-json/wp/v2/users/. Daqui, retiramos que o ID 1 pertence ao admin.
- Correr a script de ataque da seguinte forma: `./exploit_CVE-2021-34646.py http://ctf-fsi.fe.up.pt:5001/ 1`, sendo que 1 representa o ID do admin.
- Usar um dos três links gerados pelo script. Um destes dá-nos acesso ao website autenticado como admin.
- De seguida, basta usar o endereço fornecido no moodle (http://ctf-fsi.fe.up.pt:5001/wp-admin/edit.php) par aceder aos posts do admin e abrir o post privado "Message to our employees", que contém a flag `flag{4f9202a9935c542918d030418d808b5e}`.
