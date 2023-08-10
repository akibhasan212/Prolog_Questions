

% Questions:

% riffle(Xs,Ys,Zs).
% Zs is a list which contains alternating elements from Xs and Ys staring
% with an element from Xs.
% If Xs and Ys are not of the same length, Zs will contain the tail of
% the larger list.
% riffle([3,4,4],[5,6,7],[3,5,4,6,4,7])
% Testable modes:
% riffle(+,+,+), riffle(+,+,-), riffle(-,+,+), riffle(+,-,+)


% You may write your answer in the space below
%If either list is empty, then just makes sure that value of output list is the other one
%Else, takes head of both 'input' lists, and appends them to the 'output' and calls function again with the tails of the input and the appended output
riffle([],Ys,Zs) :- Ys=Zs.
riffle(Xs,[],Zs) :-  Xs=Zs.
riffle([X|Xs],[Y|Ys],[X,Y|Zs]) :- riffle(Xs,Ys,Zs).

% list_only(Xs,T,Ys).
% Given a list Xs and a term T, Ys is Xs but with only the T terms remaining in
% it.
% list_only([],5,[]) holds as there is no 5 in the empty list
% list_only([4],5,[4]) does not hold as the 4 should not be in the second list
% list_only([4],4,[4]) holds as 4 is still in the list Ys
% list_only([4,5,4],4,[4]) does not hold as it is missing a 4 from [4,5,4]
% list_only([4,5,4],4,[4,4]) holds
% Testable Modes:
% list_only(+,+,+),list_only(+,+,-),list_only(-,-,-)
%If both lists are empty all values hold
%If the T term list is empty we just check if Y is present in the big list because if not then it means it does not contain any extra T terms
%Otherwie, checks if the T term given is actually a member of the shorter list and if so then deletes one occurence of that term from both lists and calls function again with deleted lists.
%In case where the number of T terms is same in both lists then line 2 of the list_only function will be satisfied where the second input list is empty
list_only([],_,[]).
list_only(X,Y,[]) :- \+member(Y,X).
list_only(X,Y,Z) :- member(Y,Z),select(Y,X,Xs),select(Y,Z,Zs),list_only(Xs,Y,Zs).



% neg_only(Xs,Ys).
% Given a list of integers Xs, Ys is Xs but with all negative integers removed
% Testable Modes:
% neg_only(+,+), neg_only(+,-)
%If first list is empty the second is returned, otherwise, at start second list is empty and head of first list is added to it if it is positive and function is called again with tail of original list and the second list
%On later iterations, head is added to second list if it is positive and then function called again with tail of first list and (un)modified second list.
neg_only([],Ys) :- Ys=Ys.
neg_only([X|Xs],[X]) :- X>=0,neg_only(Xs,[X]).
neg_only([X|Xs],[]) :- X<0,neg_only(Xs,[]).
neg_only([X|Xs],[X|Y]) :- X>=0,neg_only(Xs,Y).
neg_only([X|Xs],Y) :- X<0,neg_only(Xs,Y).



% GTree Question
% Here is a set of facts and rules to represent a GTree:
leaf(_).
gnode([]).
gnode([leaf(_)|T]):-
    gnode(T).
gnode([gnode(X)|T]):-
    gnode(X),
    gnode(T).

% gtree_list(Tree,List).
% Write a relation between a GTree and it's corresponding list representation
% If a GTree is a leaf(X), return X
% If a GTree is a gnode(List), return List
% Examples:
% gtree_list(leaf(example),example).
% gtree_list(gnode([leaf(5),leaf(4)]),[5,4]).
% gtree_list(gnode([gnode([leaf(5)]),leaf(4)]),[[5],4]).
% gtree_list(gnode([]),[]).

% gtree_list should just return whatever is in a leaf even if it is another
% gtree:
% gtree_list(gnode([leaf(leaf(example))]),[leaf(example)]).
% Testable modes: gtree_list(+,+), gtree_list(-,+), gtree_list(+,-)
%If leaf is found then element of leaf is set to be output
%If gnode is found then list inside gnode is set to be output
gtree_list(leaf(X),List) :- List=X.
gtree_list(gnode(X),List) :- List=X.


% Natural Number Questions
% Here is a set of facts and rules to represent the natural numbers:
succ(succ(X)):-
    succ(X).
succ(nat_zero).
nat_zero.

% int_nat(Int,Nat).
% Write a relation which relates an integer to a Nat
% If the integer is negative, the program should return false
% Testable modes:
% int_nat(+,+), int_nat(+,-)

%Firstly deals with the base cases of where 0 is just nat_zero and 1 is succ(nat_zero)
%Then just proceeds to use a recurisve call where anything more than 1 is represented as
%succ(Y) and then one succ is removed from it and then it is called recursively with X-1 to find out how many succ gets removed which will be the integer number
int_nat(X,Y) :- X=:=0,Y=nat_zero.
int_nat(X,Y) :- X=:=1,Y=succ(nat_zero).
int_nat(X,succ(Y)) :- X>0,int_nat(X-1,Y).



% Here is a set of facts and rules to represent binary numbers:
bin(bin(X),bin(Y,Z)):-
    bin(X),
    bin(Y,Z).
bin(bin(X),bin(Y)):-
    bin(X),
    bin(Y).
bin(zero).
bin(one).
% So for example, 10010 becomes: bin(bin(one),bin(bin(zero),bin(bin(zero),bin(bin(one),bin(zero)))))

% bin_nat(Bin,Nat).
% Write a relation that relates an unsigned binary number to it's
% natural number representation. You may use int_nat if it helps you
% but remember to test your solution going bin to nat and nat to bin.

% Note: If you are in SSH and swipl does not show the full answer
% try to input this into the swipl repl:
% set_prolog_flag(answer_write_options,[max_depth(0)]).

% Hint: When testing, don't forget to use nat_zero for natural numbers!
% Hint: Whenever you add a zero on the right hand side of a binary number you
%       multiply by 2
% Hint: You will likely have to create multiple relations to solve this
% Hint: Try reordering your rules and utilizing a cut to make bin_nat(-,+) work

% Examples:
% bin_nat(bin(zero),nat_zero).
% bin_nat(bin(bin(one),bin(one)),succ(succ(succ(nat_zero)))).
% Note that you must be able to deal with trailing zeroes like so:
% bin_nat(bin(bin(zero),bin(bin(zero),bin(bin(one),bin(one)))),succ(succ(succ(nat_zero)))).
% Testable modes: bin_nat(+,+), bin_nat(+,-), bin_nat(-,+)
bin_nat(Bin,Nat) :- Bin = bin(bin(one),bin(zero)), Nat = succ(succ(nat_zero)).
bin_nat(Bin,Nat) :- Bin = bin(bin(one),bin(one)), Nat = succ(succ(succ(nat_zero))).
bin_nat(Bin,Nat) :- Bin = bin(zero), Nat = nat_zero.
bin_nat(Bin,Nat) :- Bin = bin(one), Nat = succ(nat_zero).
bin_nat(Bin,succ(Nat)) :- Bin = bin(bin(one),bin(Bi)), bin_nat(bin(Bi),succ(succ(Nat))).
bin_nat(Bin,succ(Nat)) :- Bin = bin(bin(zero),bin(Bi)),  bin_nat(bin(Bi),succ(Nat)).
