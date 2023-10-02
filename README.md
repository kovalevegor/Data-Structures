@startuml Library System
title Library

' left to right direction
skinparam packageStyle rect

actor "Client" as Client
actor :Librarian: as Librarian
actor :Accounting System: as AccSys
actor Time as Time  
' actor :Client DB: as CDB

Librarian -left-|> Client
rectangle LibrarySystem {
usecase ToRegisterate as Registrate
usecase ToUpdateClientData as Update
usecase ToReturnABook as Return 
usecase ToTakeOutABook as Take
usecase ToPayForServices as Payment
usecase ToCheckDeadline as Deadline
usecase ToImposeAFine as Fine
usecase ToMakeADiscount as Discount
usecase ToInspectABook as Inspect
usecase ToCheckOutNews as Inform
usecase ToOrderABook as Order
usecase ToCheckTakenBook as Check
}

Client -- Return
Client -- Take
Client -- Order
Librarian -- Registrate
Client -- Inform

Librarian -- Update
Librarian -- Inspect
Librarian -- Check

' Update .down.> Return: includes
Update <.up. Take: extends
Time -down- Deadline
' Update .up.> Fine: includes
Update <.down. Return: extends
Update <.up. Order : extends
' Update .left. Registrate : includes
Registrate <.down. Discount : extends
Update <.down. Inform : extends
' Time -- Discount
' Time -- Update
' Update .down.> Inspect : includes

Check ..> Take :extends
Discount .down.> Payment :extends
Inspect <.down. Fine : extends
Deadline <.down. Fine :extends
Fine ..> Payment :extends
' Fine .down.> Payment :extemds
Return .right.> Payment :includes

AccSys -up- Payment


@enduml


' Требуется разработать систему для абонемента библиотеки. Библиотека зарабатывает деньги, выдавая напрокат некоторые книги, имеющиеся в небольшом количестве экземпляров. Система должна отслеживать финансовые показатели работы библиотеки.

' У каждой выдаваемой в прокат книги есть название, автор, жанр. В зависимости от ценности книги определена для каждой из них залоговая стоимость (сумма, вносимая клиентом при взятии книги напрокат) и стоимость проката (сумма, которую клиент платит при возврате книги, получая назад залог). Стоимость проката книги зависит не только от самой книги, но и от срока ее проката.

' В библиотеку обращаются читатели. Все читатели регистрируются в картотеке, которая содержит стандартные анкетные данные (ФИО, адрес, телефон, категория). Каждый читатель может обращаться в библиотеку несколько раз. Все обращения читателей фиксируются, при этом по каждому факту выдачи книги (при наличии экземпляра) фиксируется дата выдачи и ожидаемая дата возврата.

' Кроме того, в системе должна быть реализована система штрафов за нанесенный книге вред и система скидок для некоторых категорий читателей. Также в системе должно быть реализовано отслеживание заказов читателей (сроков их исполнения и возврата книг), информирование читателей о новых поступлениях и просроченных книгах.
