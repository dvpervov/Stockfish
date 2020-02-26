cmake_minimum_required(VERSION 3.1)
project(stockfish)

set(CMAKE_CXX_STANDARD 14)
find_package(Threads REQUIRED)

include_directories(syzygy)

set(NNUE ON)
add_definitions(-DEVAL_NNUE)

set(HEADERS
  bitboard.h
  endgame.h
  evaluate.h
  material.h
  misc.h
  movegen.h
  movepick.h
  pawns.h
  position.h
  search.h
  thread.h
  thread_win32_osx.h
  timeman.h
  tt.h
  types.h
  uci.h
  syzygy/tbprobe.h
)

set(SOURCES
  benchmark.cpp
  bitbase.cpp
  bitboard.cpp
  endgame.cpp
  evaluate.cpp
  main.cpp
  material.cpp
  misc.cpp
  movegen.cpp
  movepick.cpp
  pawns.cpp
  position.cpp
  psqt.cpp
  search.cpp
  thread.cpp
  timeman.cpp
  tt.cpp
  uci.cpp
  ucioption.cpp
  syzygy/tbprobe.cpp
)

if(NNUE)
include_directories(learn)
include_directories(eval)
set(HEADERS
    ${HEADERS}
    learn/half_float.h
    learn/learn.h
    learn/learning_tools.h
    learn/multi_think.h
    eval/evaluate_common.h
    eval/evaluate_mir_inv_tools.h
    eval/nnue/evaluate_nnue.h
    eval/nnue/evaluate_nnue_learner.h
    eval/nnue/nnue_accumulator.h
    eval/nnue/nnue_architecture.h
    eval/nnue/nnue_common.h
    eval/nnue/nnue_feature_transformer.h
    eval/nnue/nnue_test_command.h
    eval/nnue/architectures/halfkp_256x2-32-32.h
    eval/nnue/architectures/halfkp-cr-ep_256x2-32-32.h
    eval/nnue/architectures/k-p_256x2-32-32.h
    eval/nnue/architectures/k-p-cr_256x2-32-32.h
    eval/nnue/architectures/k-p-cr-ep_256x2-32-32.h
    eval/nnue/features/castling_right.h
    eval/nnue/features/enpassant.h
    eval/nnue/features/features_common.h
    eval/nnue/features/feature_set.h
    eval/nnue/features/half_kp.h
    eval/nnue/features/half_relative_kp.h
    eval/nnue/features/index_list.h
    eval/nnue/features/k.h
    eval/nnue/features/p.h
    eval/nnue/layers/affine_transform.h
    eval/nnue/layers/clipped_relu.h
    eval/nnue/layers/input_slice.h
    eval/nnue/layers/sum.h
    eval/nnue/trainer/trainer_affine_transform.h
    eval/nnue/trainer/trainer_clipped_relu.h
    eval/nnue/trainer/trainer_feature_transformer.h
    eval/nnue/trainer/trainer.h
    eval/nnue/trainer/trainer_input_slice.h
    eval/nnue/trainer/trainer_sum.h
    eval/nnue/trainer/features/factorizer_feature_set.h
    eval/nnue/trainer/features/factorizer.h
    eval/nnue/trainer/features/factorizer_half_kp.h
  )
 set(SOURCES
    ${SOURCES}
    extra/sfen_packer.cpp
    learn/gensfen2019.cpp
    learn/learner.cpp
    learn/learning_tools.cpp
    learn/multi_think.cpp
    eval/evaluate_mir_inv_tools.cpp
    eval/nnue/evaluate_nnue.cpp
    eval/nnue/evaluate_nnue_learner.cpp
    eval/nnue/nnue_test_command.cpp
    eval/nnue/features/castling_right.cpp
    eval/nnue/features/enpassant.cpp
    eval/nnue/features/half_kp.cpp
    eval/nnue/features/half_relative_kp.cpp
    eval/nnue/features/k.cpp
    eval/nnue/features/p.cpp
  )
endif()

add_executable(${PROJECT_NAME} ${HEADERS} ${SOURCES})
target_link_libraries(${PROJECT_NAME} Threads::Threads)
