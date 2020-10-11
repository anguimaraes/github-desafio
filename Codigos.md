DELETE FROM atende.solicitacao
WHERE CTID NOT IN(
SELECT MIN (CTID)
FROM atende.solicitacao
GROUP BY protocolo
);

-------------------------------------------------------------------------------------------------------------------------------------------

CREATE FUNCTION GERA_PROTOCOLO()
RETURNS TRIGGER AS $$
BEGIN
	INSERT INTO atende.solicitacao(protocolo)VALUES(DEFAULT);
RETURN NEW;
END;
$$
LANGUAGE PLPGSQL;

CREATE TRIGGER GR_PROTOCOLO
AFTER INSERT ON atende.solicitacao
FOR EACH ROW
	EXECUTE PROCEDURE GERA_PROTOCOLO();

-------------------------------------------------------------------------------------------------------------------------------------------

CREATE FUNCTION PROTOCOLO_MOBA(protocolo_mobile INT)
RETURNS INT AS $$
	RETURN protocolo_mobile;
END
$$ LANGUAGE PLPGSQL;
