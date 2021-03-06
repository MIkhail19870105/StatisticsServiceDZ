# Домашнее задание к занятию «Выстраивание процесса непрерывной интеграции (CI): Github Actions. Покрытие кода с JaCoCo, статический анализ кода: CheckStyle, SpotBugs»

# Задача 1

## Счетчики покрытия
JaCoCo использует набор различных счетчиков для расчета показателей покрытия. 
Все эти счетчики получены из информации, содержащейся в файлах классов Java, которые в основном являются инструкциями байт-кода Java и информацией отладки, необязательно встроенной в файлы классов. 
Такой подход позволяет эффективно использовать инструменты и анализировать приложения на лету, даже если исходный код недоступен. В большинстве случаев собранная информация может быть преобразована обратно в исходный код и визуализирована до уровня детализации строки. 
Во всяком случае, у этого подхода есть ограничения. Файлы классов должны быть скомпилированы с отладочной информацией для расчета покрытия на уровне линии и обеспечения выделения источника. Не все конструкции языка Java могут быть напрямую скомпилированы в соответствующий байт-код. 
В таких случаях компилятор Java создает так называемыесинтетический код, который иногда приводит к неожиданным результатам покрытия кода.

## INSTRUCTION
Наименьшая единица измерения JaCoCo - это инструкции Java-байт-кода. Охват инструкций предоставляет информацию о количестве кода, который был выполнен или пропущен. 
Этот показатель полностью независим от исходного форматирования и всегда доступен даже при отсутствии отладочной информации в файлах классов.

## BRANCH
JaCoCo также рассчитывает покрытие веток для всех if и switch операторов. 
Эта метрика подсчитывает общее количество таких ветвей в методе и определяет количество выполненных или пропущенных ветвей. 
Покрытие веток всегда доступно, даже в отсутствие отладочной информации в файлах классов. 
Обратите внимание, что обработка исключений не рассматривается как ветви в контексте этого определения счетчика.

Если файлы классов не были скомпилированы с отладочной информацией, точки принятия решения могут быть сопоставлены с исходными строками и выделены соответствующим образом:

* Нет покрытия: не было выполнено ни одной ветки в строке (красный ромб)
* Частичное покрытие: была выполнена только часть ветвей в линии (желтый ромб)
* Полный охват: все ветки в линии были выполнены (зеленый бриллиант)

# LINE

Для всех файлов классов, которые были скомпилированы с отладочной информацией, можно рассчитать информацию покрытия для отдельных строк. 
Строка источника считается выполненной, когда выполнена хотя бы одна инструкция, назначенная этой строке.

В связи с тем, что одна строка обычно компилируется в инструкции из нескольких байт-кода, выделение исходного кода показывает три разных состояния для каждой строки, содержащей исходный код:

* Нет покрытия: в строке не было выполнено ни одной инструкции (красный фон)
* Частичное покрытие: только часть инструкции в строке была выполнена (желтый фон)
* Полный охват: все инструкции в строке были выполнены (зеленый фон)

В зависимости от исходного форматирования одна строка исходного кода может ссылаться на несколько методов или несколько классов. 
Поэтому счетчик строк методов не может быть просто добавлен, чтобы получить общее число для содержащего класса. 
То же самое относится и к строкам нескольких классов в одном исходном файле. 
JaCoCo рассчитывает покрытие строк для классов и исходного файла на основе фактических исходных покрытых строк.

Использовал покрытие **LINE** как усредненный вариант. По сути первый тест который был в задаче, не выполнял все строки кода так как не было выполнено условие current_max < income 
из-за этого JaCoCo показывал покрытие тестами не на 100%. Добавив тест с другими тестовыми данными в которых первое значение не является максимальным, было выполнено условие current_max < income 
соответственно тест прошел большим количеством циклов и JaCoCo показал 100% покрытие тестами.

# Задача 2

```xml
<plugin>
	<groupId>com.github.spotbugs</groupId>	
	<artifactId>spotbugs-maven-plugin</artifactId>	
	<version>4.0.0</version>	
		<executions>
			<execution>			
				<id>check</id>				
				<phase>verify</phase>				
				<goals>				
				<goal>check</goal>				
				</goals>				
			</execution>			
		</executions>		
</plugin>


# Задача 3

Сборка падает так как код неудовлетворяет стандартам кодирования.

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-checkstyle-plugin</artifactId>
    <version>3.1.1</version>
        <configuration>
           <configLocation>checkstyle.xml</configLocation>
              <encoding>UTF-8</encoding>
              <consoleOutput>true</consoleOutput>
              <failsOnError>true</failsOnError>
              <linkXRef>false</linkXRef>
        </configuration>
            <executions>
                <execution>
                    <id>check</id>
                    <phase>verify</phase>
                    <goals>
                       <goal>check</goal>
                    </goals>
                 </execution>
            </executions>
</plugin>
