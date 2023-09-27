# banco-de-dados-2
DELIMITER //
CREATE PROCEDURE sp_LivrosPorCategoria(IN nome_categoria VARCHAR(225))
BEGIN
	SELECT l.Titulo, c.Nome AS Categoria
    FROM Livro l
    INNER JOIN Categoria c ON li.Categoria_ID = c.Categoria_ID
    WHERE ca.Nome = nome_categoria;
END;

-- exercício 3
DELIMITER //
CREATE PROCEDURE sp_ContarLivrosPorCategoria(IN nome_categoria VARCHAR(225))
BEGIN
	SELECT ca.Nome, COUNT(l.Titulo) AS quantidade_livros
    FROM Livro l
    INNER JOIN Categoria c ON li.Categoria_ID = c.Categoria_ID
    WHERE ca.Nome = nome_categoria
    GROUP BY ca.Nome;
END;

-- exercício 4

DELIMITER //
CREATE PROCEDURE sp_VerificarLivrosCategoria(IN nome_categoria VARCHAR(225), OUT possui_livros ENUM('Sim possui', 'Não possui'))
BEGIN
    DECLARE quantidade_livros INT;
	SELECT COUNT(l.Categoria_ID) AS quantidade_livros
    INTO quantidade_livros
    FROM Categoria c
    INNER JOIN Livro l ON c.Categoria_ID = l.Categoria_ID
    WHERE ca.Nome = nome_categoria
    GROUP BY ca.Nome;
   
    IF quantidade_livros > 0 THEN
		SET possui_livros = 'Sim possui';
	ELSE
		SET possui_livros = 'Não possui';
	END IF;
END;
//
DELIMITER ;

CALL sp_VerificarLivrosCategoria('Romance', @possui_livros);
SELECT @possui_livros;


-- exercício 5

CREATE PROCEDURE sp_LivrosAteAno INT
BEGIN
    SELECT  l.Titulo, l.Ano_Publicacao
    FROM Livro l
    WHERE l.Ano_Publicacao <= ano
END;
//
DELIMITER ;
CALL sp_LivrosAteAno(2010);

-- exercício 6

DELIMITER //
CREATE PROCEDURE sp_TitulosPorCategoria(IN nome_categoria VARCHAR(225))
BEGIN
	SELECT l.Titulo
    FROM Livro l
    INNER JOIN Categoria c ON l.Categoria_ID = c.Categoria_ID
    WHERE ca.Nome = nome_categoria;
END;
//
DELIMITER ;

CALL sp_TitulosPorCategoria('história');

-- exercício 7

DELIMITER //
CREATE PROCEDURE sp_AdicionarLivro(IN nome_livro VARCHAR(250))
BEGIN
	DECLARE por_livros INT;
    SELECT l.Livro_ID 
    INTO por_livros
    FROM Livro l
    WHERE l.Titulo = nome_livro   
    IF por_livros IS NOT NULL THEN
		SIGNAL SQLSTATE '45000'
		SET MESSAGE_TEXT = 'Livro existente';
	ELSE
		INSERT INTO Livro (Titulo)
		VALUES (nome_livro);
	END IF;
END;
//
DELIMITER ;

CALL sp_AdicionarLivro('As Vantagens de ser invisível');
SELECT l.Titulo FROM Livro l;

-- exercício 8

DELIMITER //
CREATE PROCEDURE sp_autorMaisAntigo()
BEGIN
	SELECT Nome, Sobrenome
    FROM Autor
    WHERE Data_Nascimento = (
		SELECT MIN(Data_Nascimento)
        FROM Autor);
END;
//
DELIMITER ;

CALL sp_autorMaisAntigo();
