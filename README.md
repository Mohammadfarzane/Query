SELECT id, score,
       score * 100.0 / SUM(score) OVER () AS score_percent
FROM students;

SELECT *
FROM prices
WHERE price < LAG(price) OVER (ORDER BY created_at);

DELETE FROM users
WHERE id IN (
    SELECT id FROM (
        SELECT id,
               ROW_NUMBER() OVER (PARTITION BY email ORDER BY id) rn
        FROM users
    ) t
    WHERE rn > 1
);

SELECT id,
       DATEDIFF(end_date, start_date) AS days_diff
FROM projects;

SELECT *
FROM students s
WHERE score > (
    SELECT AVG(score)
    FROM students
    WHERE major = s.major
);

SELECT ROW_NUMBER() OVER (ORDER BY created_at) AS serial_no, *
FROM logs;

SELECT *
FROM employees
WHERE name IS NULL OR salary IS NULL OR dept_id IS NULL;

SELECT student_id, subject, score
FROM scores
CROSS APPLY (
    VALUES
    ('Math', math),
    ('Physics', physics),
    ('Chemistry', chemistry)
) v(subject, score);

SELECT DISTINCT major,
       MAX(score) OVER (PARTITION BY major) AS max_score
FROM students;

SELECT order_id, order_date, amount,
       SUM(amount) OVER (ORDER BY order_date) AS running_total
FROM orders;
