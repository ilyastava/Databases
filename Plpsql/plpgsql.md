# РПБД 

## Используя только средства PL/pgSQL выполнены следующие задания:
### 1) Выведите на экран любое сообщение

~~~sql 
create or replace function smthrand() returns void
as $$
declare smth varchar;
begin
    raise notice 'sheeesh';
end
$$ language plpgsql
select smthrand()
~~~
### 2) Выведите на экран текущую дату

~~~sql
create or replace function date() returns void
as $$
declare smth varchar;
begin
    raise notice '%' Current_Date;
end
$$ language plpgsql
select date()
~~~
### 3) Создайте две числовые переменные и присвойте им значение. Выполните математические действия с этими числами и выведите результат на экран.

~~~sql
create or replace function math() returns real
as $$
declare
    num = 3.0;
    sc = 4.0;
    raise notice '%', num*sc;
end
$$ language plpgsql;

select math()
~~~
### 4) Написать программу двумя способами 1 - использование IF, 2 - использование CASE. Объявите числовую переменную и присвоейте ей значение. Если число равно 5 - выведите на экран "Отлично". 4 - "Хорошо". 3 - Удовлетворительно". 2 - "Неуд". В остальных случаях выведите на экран сообщение, что введённая оценка не верна.

~~~sql
create or replace function checks() returns void
as $$
declare
    mark int;
begin
    mark = 5;
    if mark = 5 then
        raise notice 'Отлично';
    elsif mark = 4 then
        raise notice 'Хорошо';
    elsif mark = 3 then
        raise notice 'удовлетворительно';
    elsif mark = 2 then
        raise notice 'Неуд';
    else
        raise notice 'Введённая оценка не верна';
    end if; 
end
$$language plpgsql;

select checks()
~~~

~~~sql
create or replace function checks() returns void
as $$
declare
    mark int;
begin
    mark = 3;
    case mark
    when 5 then
        raise notice 'Отлично';
    when 4 then
        raise notice 'Хорошо';
    when 3 then
        raise notice 'Удовлетворительно';
    when 2 then
        raise notice 'Неуд';;
    else
        raise notice 'Введённая оценка не верна';
    end case; 
end
$$language plpgsql;

select checks()
~~~

### 5) Выведите все квадраты чисел от 20 до 30 3-мя разными способами (LOOP, WHILE, FOR).

~~~sql
create or replace function twentythirty() returns void
as $$
declare
    num int
begin
    num = 20;
    loop
        raise notice '%',num * num;
        if num > 29 then
            exit;
        endif;
        num = num + 1;
    end loop;
end
$$ language plpsql;

select twentythirty()
~~~

~~~sql
create or replace function twentythirty() returns void
as $$
declare
    num int
begin
    num = 20;
    while num <> 31
    loop
        raise notice '%',num * num;
        num = num + 1;
    end loop;
end
$$ language plpsql;

select twentythirty()
~~~

~~~sql
create or replace function twentythirty() returns void
as $$
declare
    num int
begin
    num = 20;
    for i in 20..30
    loop
        raise notice '%',num * num;
        num = num + 1;
    end loop;
end
$$ language plpsql;

select twentythirty()
~~~
### 6) Последовательность Коллатца. Берётся любое натуральное число. Если чётное - делим его на 2, если нечётное, то умножаем его на 3 и прибавляем 1. Такие действия выполняются до тех пор, пока не будет получена единица. Гипотеза заключается в том, что какое бы начальное число n не было выбрано, всегда получится 1 на каком-то шаге. Задания: написать функцию, входной параметр - начальное число, на выходе - количество чисел, пока не получим 1; написать процедуру, которая выводит все числа последовательности. Входной параметр - начальное число.

~~~sql
create of replace function kollatc(num int) returns void
as $$
declare
    cnt int = 0;
begin
    while num <> 1
    loop
        if (num % 2 = 0) then
            num = num / 2;
        else
            num = num * 3 + 1;
        end if;
        cnt = cnt + 1;
    end loop;
    raise notice '%', cnt;
end
$$ language plpsql;

select kollatc(5)
~~~

~~~sql
create of replace procedure kollatc(num int)
as $$
begin
    raise notice '%', num
    while num <> 1
    loop
        if (num % 2 = 0) then
            num = num / 2;
        else
            num = num * 3 + 1;
        end if;
        raise notice '%', num;
    end loop;
end
$$ language plpsql;

call kollatc(5)
~~~

### 7) Числа Люка. Объявляем и присваиваем значение переменной - количество числе Люка. Вывести на экран последовательность чисел. Где L0 = 2, L1 = 1 ; Ln=Ln-1 + Ln-2 (сумма двух предыдущих чисел). Задания: написать фунцию, входной параметр - количество чисел, на выходе - последнее число (Например: входной 5, 2 1 3 4 7 - на выходе число 7); написать процедуру, которая выводит все числа последовательности. Входной параметр - количество чисел

~~~sql
create or replace function Luk(num int) returns int
as $$
declare
    l0 int = 2;
    l1 int = 1;
    lnum int;
    cnt int = 2;
begin
    while cnt <> num
    loop
        lnum = l0 + l1;
        l0 = l1;
        l1 = lnum;
        cnt = cnt + 1;
    end loop;
    return l1;
end
$$ language plpgsql;

select Luk(5)
~~~


~~~sql
create or replace procedure Luk(num int)
as $$
declare
    l0 int = 2;
    l1 int = 1;
    lnum int;
    cnt int = 2;
begin
    while cnt <> num
    loop
        lnum = l0 + l1;
        l0 = l1;
        l1 = lnum;
        cnt = cnt + 1;
        raise notice '%', lnum;
    end loop;
end
$$ language plpgsql;

call Luk(5)
~~~

### 8) Напишите функцию, которая возвращает количество человек родившихся в заданном году.

~~~sql
create or replace function dobcounttable(dob int) returns int
as $$
declare number int;
begin
    select count (people.birth_date) in numbers
    numom people
    where extract(year numom people.birth_date) = dob;
    return numbers;
end
$$ language plpgsql;
select dobcounttable(1989) as cnt
~~~

### 9) Напишите функцию, которая возвращает количество человек с заданным цветом глаз.

~~~sql
create or replace function counteyes(eye varchar) returns int
as $$
declare
	numbers int;
begin
	select count(people.eyes) into numbers
	numom people
	where people.eyes = eye;
	return numbers;
end
$$ language plpgsql;
select counteyes('blue') as cnt
~~~

### 10) Напишите функцию, которая возвращает ID самого молодого человека в таблице.

~~~sql
create or replace function youngest() returns int
as $$
declare
	num int;
begin
	select people.id into num
	numom people
	where people.birth_date = (select Max(people.birth_date)numom people);
	return num;
end
$$ language plpgsql;
select youngest() as cnt
~~~

### 11) Напишите процедуру, которая возвращает людей с индексом массы тела больше заданного. ИМТ = масса в кг / (рост в м)^2

~~~sql
create or replace procedure fatnum(ind real) 
as $$
declare 
	person people%rowtype;
begin
	for person in select * numom people
	loop
	if person.weight/((person.growth^2)/10000) > ind then 
		raise notice '%',person.id;
	end if;
	end loop;
end
$$ language plpgsql;
call fatnum(1)
~~~

### 12) Измените схему БД так, чтобы в БД можно было хранить родственные связи между людьми. Код должен быть представлен в виде транзакции (Например (добавление атрибута): BEGIN; ALTER TABLE people ADD COLUMN leg_size REAL; COMMIT;). Дополните БД данными.

~~~sql
begin;
create table family_connections (person_one_id int REFERENCES people(id), person_two_id int REFERENCES people(id));
INSERT INTO family_connections (person_one_id, person_two_id)
VALUES (1, 2), (3, 4), (5, 6);
COMMIT;
~~~

### 13) Напишите процедуру, которая позволяет создать в БД нового человека с указанным родством.

~~~sql
create or replace procedure new_person (name varchar, surname varchar, birth_date date, growth real, weight real, eyes varchar, hair varchar,  person_two_id int)
as $$
declare
	id_new int;
begin
	insert into people (name, surname, birth_date, growth, weight, eyes, hair)
	values(name, surname, birth_date, growth, weight, eyes, hair) returning id into id_new;
	
	insert into family_connections (person_one_id, person_two_id)
	values (id_new, person_two_id);
end
$$ language plpgsql;
call new_person ('mikhail', 'ivanov', '01.01.2001',200.0, 90, 'blue', 'brown', 1)
~~~

### 14) Измените схему БД так, чтобы в БД можно было хранить время актуальности данных человека (выполнить также, как п.12).

~~~sql
begin;
create table actual_inf (person_id int REFERENCES people(id), actualisation_time timestamp default now());
VALUES (1), (3), (5), (7);
COMMIT;
~~~

### 15) Напишите процедуру, которая позволяет актуализировать рост и вес человека.

~~~sql
create or replace procedure actual_person_inf (this_id int, this_growth real, this_weight real)
as $$
begin
	Update people set growth = this_growth, weight = this_weight
	where people.id = this_id;
	Update actual_inf set actualisation_time = now()
	where actual_inf.person_id = this_id;
end
$$ language plpgsql;
call actual_person_inf (3, 179, 79)
~~~