use std::io;

enum MoveResult {
    FailedNotExist,
    FailedOccupied,
    Success
}

enum MatchResult {
    WinX,
    WinO,
    Draw,
    InProgress
}

#[derive(Clone, Copy)]
struct Field {
    Id: u8,
    Mark: char
}

#[derive(Clone, Copy)]
struct Player {
    Mark: char
}

fn main() {
    println!("Welcome to TicTacToe written in Rust!");

    let mut board: Vec<Field> = Vec::with_capacity(9);

    for i in 1..10 {
        board.push(Field {
            Id: i,
            Mark: ' '
        });
    }

    let player1: Player = Player { Mark: 'X' };
    let player2: Player = Player { Mark: 'O' };

    let mut current_turn: &Player = &player1;

    draw_board(&board);

    loop {
        let mut field_input: String = String::new();

        println!("It's {} turn!", &current_turn.Mark);

        io::stdin()
            .read_line(&mut field_input);
        
        match check_move(&board, &field_input) {
            MoveResult::FailedNotExist => {
                println!("Input a number 1 - 9!");
                continue;
            },

            MoveResult::FailedOccupied => {
                println!("Selected field is already occupied!");
                continue;
            },

            MoveResult::Success => {
                print!("{esc}c", esc = 27 as char);

                let field_id: usize = field_input.trim().parse::<usize>().unwrap();
                make_move(&mut board, current_turn, field_id);
                draw_board(&board);

                match get_match_result(&board) {
                    MatchResult::InProgress => current_turn = if (current_turn.Mark == player1.Mark) { &player2 } else { &player1 },

                    MatchResult::Draw => {
                        println!("It's a draw!");
                        break;
                    }

                    MatchResult::WinO => {
                        println!("Woohoo! The winner is O!");
                        break
                    }

                    MatchResult::WinX => {
                        println!("Woohoo! The winner is X!");
                        break
                    }
                }
            }
        }
    }
}

fn get_match_result(board: &Vec<Field>) -> MatchResult {
    let winning_configs = [
        (0, 1, 2),
        (0, 3, 6),
        (3, 4, 5),
        (1, 4, 7),
        (6, 7, 8),
        (2, 5, 8),
        (0, 4, 8),
        (2, 4, 6)
    ];

    for conf in winning_configs.iter() {
        if (board[conf.0].Mark != ' ' && (board[conf.0].Mark == board[conf.1].Mark) && board[conf.1].Mark == board[conf.2].Mark) {
            return if (board[conf.0].Mark == 'X') { MatchResult::WinX } else { MatchResult::WinO };
        }
    }

    if (is_draw(board)) {
        return MatchResult::Draw;
    }

    MatchResult::InProgress
}

fn is_draw(board: &Vec<Field>) -> bool {
    for field in board.iter() {
        if (field.Mark == ' ') { return false; }
    }
    
    true
}

fn check_move(board: &Vec<Field>, field_input: &String) -> MoveResult {
    let field_id = match field_input.trim().parse::<usize>() {
        Ok(val) => val,
         _ => return MoveResult::FailedNotExist
    };

    if (field_id <= 0 || field_id >= 10) { return MoveResult::FailedNotExist; }

    let field = &board[&field_id - 1];
    if (field.Mark != ' ') { return MoveResult::FailedOccupied; }

    MoveResult::Success
}

fn make_move(board: &mut Vec<Field>, player: &Player, field_id: usize) -> () {
    board[field_id - 1].Mark = player.Mark;
}

fn draw_board(board: &Vec<Field>) -> () {
    for i in 0..3 {
        println!("{}   {}   {}", board[(i * 3)].Mark, board[(i * 3) + 1].Mark, board[(i * 3) + 2].Mark);
        println!("----------")
    }
}
