WIN_COMBINATIONS = [[0,1,2], [3,4,5], [6,7,8], [0,3,6], [1,4,7], [2,5,8], [0,4,8], [2,4,6]]

def initialize
  @board = Array.new(9, " ")
end

def display_board
  puts " #{@board[0]} | #{@board[1]} | #{@board[2]} "
  puts "-----------"
  puts " #{@board[3]} | #{@board[4]} | #{@board[5]} "
  puts "-----------"
  puts " #{@board[6]} | #{@board[7]} | #{@board[8]} "
end

def input_to_index(user_input)
  user_input.to_i - 1
end

def move(index, current_player="X")
  @board[index] = current_player
end

def valid_move?(position)
  position.between?(0,8) && !position_taken?(position)
end

def position_taken?(position)
  ![" ", "", nil].include?(@board[position])
end

def turn
  puts "Please enter 1-9:"
  user_input = gets.strip
  index = input_to_index(user_input)
  if valid_move?(index)
    move(index, current_player)
    display_board
  else
    turn
  end
end

def current_player
  turn_count.even? ? "X" : "O"
end

def turn_count
  count = 0
  @board.each do |position|
    if ["X", "O"].include?(position)
      count += 1
    end
  end
  count
end

def won?
  WIN_COMBINATIONS.find do |combo|
    combo.all? {|i| @board[i] == "X"} || combo.all? {|i| @board[i] == "O"}
  end
end

def full?
  @board.all? {|pos| ["X", "O"].include?(pos)}
end

def draw?
  full? && !won?
end

def over?
  won? || draw? || full?
end

def winner
  @board[won?[0]] if won?
end

def play
  turn until over?
  if won?
    puts "Congratulations #{winner}!"
  elsif draw?
    puts "Cat's Game!"
  end
end