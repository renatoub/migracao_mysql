CREATE `sp_migracao`(
	IN value VARCHAR(255),
	IN field_ VARCHAR(255), 
	IN field_complement VARCHAR(255),
	IN value_complement VARCHAR(255),
	IN table_name VARCHAR(40)
	)
BEGIN
	DECLARE min_id INT;

	SET @sql = CONCAT('SELECT MIN(id) INTO @min_id FROM ', table_name, ' WHERE UPPER(TRIM(REGEXP_REPLACE(', field_, ', '' +'', '' ''))) = UPPER(TRIM(REGEXP_REPLACE(''', value, ''', '' +'', '' '')));');
	PREPARE stmt FROM @sql;
    EXECUTE stmt;
	SELECT  @min_id into min_id;
    DEALLOCATE PREPARE stmt;

	IF min_id IS NULL THEN
		
		set @sql = CONCAT('INSERT INTO ', table_name, ' (',field_ ,field_complement ,') VALUES(UPPER(TRIM(REGEXP_REPLACE(''', value, ''', '' +'', '' '')))' , value_complement, ''' );');
		PREPARE stmt FROM @sql;
		EXECUTE stmt;
		DEALLOCATE PREPARE stmt;
		
		SET @sql = CONCAT('SELECT MIN(id) INTO @min_id FROM ', table_name, ' WHERE UPPER(TRIM(REGEXP_REPLACE(', field_, ', '' +'', '' ''))) = UPPER(TRIM(REGEXP_REPLACE(''', value, ''', '' +'', '' '')));');
		PREPARE stmt FROM @sql;
	    EXECUTE stmt;
		SELECT  @min_id into min_id;
	    DEALLOCATE PREPARE stmt;
	END IF;
  
   SELECT min_id;
END
