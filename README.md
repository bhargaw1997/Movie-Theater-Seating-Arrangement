# Movie-Theatre-Seating-Arrangement
Program to implement seating arrangement in a movie theatre to maximize both customer satisfaction and safety.

Summary: 

This method assigns seats to customers in such a way that they are satisfied, while also maintaining public safety by giving a three-seat buffer between individual bookings.

Description: 

The algorithm must distribute seats to customers in a theatre with a seating capacity of 20 seats in each of the 10 rows (200 seats) to maximize customer safety and ensure customer satisfaction.

Input: 

Each reservation request would have one line of input in the input file. The order of the lines in the file corresponds to the receipt of the reservation requests. Each line in the file will begin with a reservation identification, followed by a space, and then the requested number of seats. R#### will be the format of the reservation identification.

Example: 
R001 2 
R002 4 
R003 4 
R004 3

Output: 

The application should generate a file containing each request's seating arrangements. The reservation number should be followed by a space, followed by a comma-delimited list of the allotted seats in each row of the file.Example: 
R001 I1,I2
R002 F16,F17,F18,F19 
R003 A1,A2,A3,A4 
R004 J4,J5,J6

Please Note: There are no seats available for the bookings that could not be accommodated at the theatre.
example: R005 0 indicates that the reservation is not able to be fulfilled at this time.

Approach to solution: 

This is a form of constraint satisfaction problem in which a collection of items must satisfy a set of constraints or limitations. Constraint satisfaction methods are used to solve CSPs, which express the entities in a problem as a homogeneous set of finite constraints over variables.
 Constraint Sattisfaction Problem Definition: Constraint satisfaction problems (CSPs) are mathematical problems where one must find states or objects that satisfy a number of constraints or criteria.

Assumptions made while implementing the Algorithm:

1. The price of all seats in the theatre is the same.
2. Seating is assigned on a first-come, first-served basis.
3. Customers who reserve seats early are given better seats (seats that are further away from the screen) than those who reserve later.
4. In order to follow customer safety, there is 3 seats buffer between individual booking.
5. When there are no empty rows and only vacant seats need to be filled, the reservation that best satisfies both satisfactions is chosen first.  
6. Customer will not get seats if number of continous seats are less than requested seats.
7. Customer can not book more than 20 seats.

Approach to the implement the solution: 

1. The algorithm employes Greedy approach with Priority Queue implemented using Linked List.
2. Each Row is a node with the following properties: name, totalSeats, pointer to the next node, seatsOccupied, and seatsEmpty.
3. When the number of seats to be reserved exceeds the number of adjacent seats in the Linked List, new rows (I,H,G....A) are added. 
    If the number of seats in J is less than the number of seats requested, a new node with the name I is added to the linked list.
4. If a row is full, it is removed from the List and the following row is added in its stead. If J is full, it is removed from the tree and I is made the node.
5. If all of the rows (nodes) are full or the remaining seats in the reservation cannot be filled, the program will exit. 
 
Why Using Greedy Approach than Backtracking: 
1. This problem can also be solved using backtracking but backtracking may compromise customer sattisfaction due to high constraint enforced. The complexity for solving the problem backtracking grows with increasing nodes in the tree. 
2. In the worst case where each reservation would require 200 calculations which would only happen if 200 reservation requests are recieved each with 1 seat.
3. Linked list helps in easy insertion and deletion as compared to lists. 
 
Scope of improvement: 
1. The Linked List can be replaced with a Binary Search Tree, with the Last Row with Empty Seats as the root node. It will help to shorten the time it takes to look through the nodes. 
2. Rather than distributing the best seats to every reservation on a first-come, first-served basis, the algorithm can set a threshold for each reservation that meets Customer Satisfaction and Theatre Utilization. 
3. Bulk seat reservations (>=100) will be given higher priority than small reservations.
 
Program Files: 
1. InputFileMaker.py: Uses random library to generate random reservation requests.
2. InputFileParser.py: Parse the given input file of above format into list of reservations
3. greedySeatAllocation: Uses greedy approach to allocate seats.

 
How to run the program: 
1. Open Command prompt or terminal. 
2. Move to the directory where the program is stored. Make sure all the program files are in the same folder before running the program.
3. The program accepts only text files of the format where each line in the file will be comprised of a reservation identifier, followed by a space, and then the number of seats requested. The reservation identifier will have the format: R####. as displayed above. 
4. Ensure python module logging is present.
5. Type the following command in the  Command Prompt or Terminal. 
        python greedySeatAllocation.py *inputFilePath*
6. It takes input file path as system arrgument. 
7. The output generated is a text file, Each row in the file should include the reservation number followed by a space, and then a comma-delimited list of the assigned seats.
8. Program also generate the debug logs for event logging. 


Program Flow:

Main Method:

1.	Program will take input file path from command line argument and store in FilePath variable.
2.	inputParser function which is stored in inputParser.py file will be called and return data will be stored in data variable.
	a.	In inputParser function input txt file will be open in rad mode and each line will be split and stored in reservationList list, where reservationList store number of seats and customer Id will be the part of list
	b.	Final will return reservationList and store in data variable.
3.	Will create object of BookingTheatre as Arrangement.
4.	Will initialize not_inserted empty list 
5.	For eachReservation in data:
	a.	If(Arrangement.seatsAvailable == 0 ) (i.e. theatre  is full)
		i.	Break
	b.	If not Arrangement.varify_seats(eachReservation[1],str(eachReservation[0])) (i.e. will return true or false. If its false it will append eachReservation into not_inserted list)
		i.	Verify_seats(seatRequested,reservationID) function:
			1.	Inalize can_insert_continous_seats to False
			2.	If(seatRequested > 20)
				o	Return False
			3.	Call lookup(seatRequired) function and store result in tnode
				o	Lookup(seatRequested) function:
						If root != NIL_NODE (i.e. root node already present)
						•	Return __lookup(self.root, seatRequested) (i.e. will return tnode which is row which still has seats left)
						•	__lookup(self.root, seatRequested) function:
							o	This function will check tnode.seatsEmpty >= seatRequested: (where seatsEmpty will call tnode class method vacant_seat(seat) function which gives total number of empty seats in row.
						else:
						•	sub=tnode.subs[1]
						•	if sub != None:
							o	return self.__lookup(sub, seatRequired) (i.e. trying to find better match in other nodes)
						•	else:
							o	return None (i.e. no existing vacant node found, creating a new node)
			4.	if(tnode == None and self.totalNodes > 0)
				o	call insert(seatRequested, reservationID, seats) function:
						if self.root != NIL_NODE:
						•	lastinsert = self.__insert_node(self.root, seatRequested, self.totalNodes, reservationID)
							o	if seatRequested <= currentNode.seatEmpty: (which check current node has enough number of seats)
									currentNode.seatOccupied = currentNode.seats_Reserved(seatReserved, currentNode.reservationID)
						•	Note: This function will book the seats
						•	will create seats_assigned empty list
						•	it will iterate through seatsOccupied list and check whether seatRequested !=	0 and then self.seatsOccupied == 0 then it will store reservationID into seatsOccupiet[i] variable and reduce 1 from seatRequested variable. Also, it will append seats_assigned variable with seat number.
						•	If reservationID not in output file, it will add reservationID and join seats_assigned list else it will join with existing reservationID
						•	Finally, returns seatsOccupied variable
						currentNode.seatsEmpty = currentNode.vacant_seat(seats) (i.e. update seatsEmpty variable)
				o	elif seatRequested > currentNode.seatsEmpty: (i.e. current row has less number of seats than requested)
						if(currentNode.subs[1]): (i.e. lookup to find the best place to add new node that is will iterate and find last Node)
						•	call __insert_node function and pass next node
						elif current.sub[1]==None and totalNodes > 0: (i.e. adding a new node(row)to the linked list)
						•	self.substract_seats(seatRequested
						•	name = char(self.totalNodes + 64)
						•	currentNode.subs[1] = tnode(name,seats,seatRequested,reservationID, currentNode) (i.e. linking new node to current node)
						•	self.totalNodes -1
							o	return currentNode.sub[1] (i.e. newly created node)
						•	self.delete(self.lastinsert) (i.e. delete node from linked list if it’s full)
							o	if currentNode.seatsEmpty == 0:
						currentNode == self.root and currentNode.subs[1] is not None:
						•	(i.e. currentNode is root node and there are no more nodes in list, so return root)
						elif currentNode is not root:
						•	currentNode.parent.subs[1]=currentNode.subs[1]
						•	if currentNode.parent.subs[1] != None:
							o	currentNode.subs[1].parent = currentNode.parent
							o	return currentNode.parent
						else:
						•	self.root = nilNode
						•	return self.root
						else: (i.e. adding first node to the list)
						•	name = char(self.totalNodes + 64)
						•	self.root = tnode(name,seats,seatRequested,reservationID, None) 
						•	self.totalNodes -1
						•	self.lastinsert = self.root
						•	self.substract_seats(seatRequested)
				o	can_insert_continuous_seats = True
			5.	elif(tnode != None): (i.e. no need to add a new node reservation can be accommodated)
				o	self.substract_seats(seatRequested)
				o	tnode.seats_reserved(seatRequested, reservationID)
				o	tnode.seatsEmpty = tnode.vacant_seat(seats)
				o	parent = self.delete(tnode)
				o	can_insert_continuous_seats = True
			6.	elif(self.totalNodes is 0):
				o	return can_insert_continous_seats
			7.	return can_insert_continous_seats
		ii.	not_inserted.append(eachReservation)
	c.	initialize more_inserted empty list
	d.	sorted_not_inserted_booking = sorted(not_inserted, key = lambda x:x[1]) (i.e. sort not_inserted list)
	e.	for eachReservation in sorted_not_inserted_bookings (i.e. allocating remaining seats by splitting groups to utilize theatre)
		i.	if Arrangement.seatsAvailable == 0 (i.e. no seats)
			1.	break
		ii.	elif( eachReservation[1] > Arrangement.available): (i.e. required seats are more compare to available)
			1.	continue
		iii.	else:
		iv.	more_inserted.append(eachReservation)
		v.	Arrangement.split_insert(eachReservation[1],str(eachReservation[0])
			1.	split_insert(eachReservation[1],str(eachReservation[0]) function
				a.	self.substarct.seats(seatsRequested)
				b.	currentNode = self.root (i.e. start searching from root node)
				c.	while currentNode!=None and seatsRequested!=0:
					i.	if currentNode.seatsEmpty <= seatRequested:
						1.	currentNode.seats_reserved(currentNode.seatsEmpty, reservationID) (i.e. will reserve all available seats in row)
						2.	seatsRequested -= currentNode.vacant_seat(seats)
						3.	currentNode.seatsEmpty = currentNode.vacant_seat(seats) (i.e. update emapty seats available for currentNode)
						4.	currentNode=currentNode.subs[1]
						5.	if currentNode is not None:
							a.	self.delete(currentNode.parent)
					ii.	else: (i.e. seatsRequested is less than available seats in row)
						1.	currentNode.seats_reserved(seatsRequested, reservationID)
						2.	currentNode.seatsEmpty = currentNode.vacant_seat(seats) (i.e. update emapty seats available for currentNode)
						3.	seatsRequested -= currentNode.vacant_seat(seats)
						4.	self.delete(currentNode)
	f.	Arrangement.writing_output(data,output)
	g.	outputFilePath = os.getcwd()+’/’+’output.txt’
	h.	print(file location)



 
 
 
 
 
 
