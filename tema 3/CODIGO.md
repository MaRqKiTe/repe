import re

# Definir una lista de tokens y sus expresiones regulares correspondientes

tokens = [
    ('numeros', r'\d+'),  # Coincide con uno o más dígitos
    ('letras', r'[a-zA-Z]'),  # Coincide con cualquier letra individual (mayúscula o minúscula)
    ('minuscula', r'[a-z]'),  # Coincide con cualquier letra minúscula individual
    ('mayuscula', r'[A-Z]'),  # Coincide con cualquier letra mayúscula individual
    ('especiales', r'[!&\_|@#$^~]'),  # Coincide con cualquier carácter especial del conjunto especificado
    ('operadores', r'[=,-,*,/,=,<,>]'),  # Coincide con cualquier operador aritmético
]


def lexer(expression):
# Combinar todos los patrones de expresiones regulares de tokens en una sola alternancia
    tokens_regex = '|'.join('(?P<%s>%s)' % par for par in tokens)
    # Compilar el patrón de expresión regular combinado
    token_matcher = re.compile(tokens_regex)

    # Iterar a través de todas las coincidencias en la expresión
    for match in token_matcher.finditer(expression):
        # Obtener el tipo de token coincidente (nombre del grupo)
        tipo_token = match.lastgroup
        # Obtener el valor coincidente real
        valor_token = match.group(tipo_token)

        if tipo_token == 'SPACE':
            continue  # Omitir espacios (se puede modificar para manejar espacios si es necesario)

        yield tipo_token, valor_token


def is_strong_password(password):
    """
    Evaluates a password's strength based on complexity criteria.

    Args:
        password (str): The password to evaluate.

    Returns:
        bool: True if the password is strong, False otherwise.
    """
    

   # Requisito de longitud mínima mejorado
    min_length = 12
    has_lowercase = False
    has_uppercase = False
    has_digit = False
    has_special = False

    for token_type, token_value in lexer(password):
        if token_type == 'minuscula':
            has_lowercase = True
        elif token_type == 'mayuscula':
            has_uppercase = True
        elif token_type == 'numeros':
            has_digit = True
        elif token_type == 'especiales':
            has_special = True

    return (len(password) >= min_length and
            has_lowercase and has_uppercase and
            has_digit and has_special)


expression = "Ma1e8o* r3A/1@"
for token in lexer(expression):
    print(token)  # Imprimir tokens para demostración

  #ingresa la contrasena para verificar que cumpla con los requisitos 
password = input("Pon tu contrasena: ")
if is_strong_password(password):
    print("Tu contrasena es segura!")
else: #sigerencias para la contrasena 
    print("Tu contraseña es débil. Aquí hay algunas sugerencias de mejora:")
    print("- Utilice un mínimo de 12 caracteres.")
    print("- Incluya letras minúsculas, letras mayúsculas, números y caracteres especiales.")
