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
