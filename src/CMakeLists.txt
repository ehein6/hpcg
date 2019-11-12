set(SOURCES
    CG.cpp CG_ref.cpp TestCG.cpp ComputeResidual.cpp
    ExchangeHalo.cpp GenerateGeometry.cpp GenerateProblem.cpp
    GenerateProblem_ref.cpp CheckProblem.cpp
    OptimizeProblem.cpp ReadHpcgDat.cpp ReportResults.cpp
    SetupHalo.cpp SetupHalo_ref.cpp TestSymmetry.cpp TestNorms.cpp WriteProblem.cpp
    YAML_Doc.cpp YAML_Element.cpp ComputeDotProduct.cpp
    ComputeDotProduct_ref.cpp finalize.cpp init.cpp mytimer.cpp ComputeSPMV.cpp
    ComputeSPMV_ref.cpp ComputeSYMGS.cpp ComputeSYMGS_ref.cpp ComputeWAXPBY.cpp ComputeWAXPBY_ref.cpp
    ComputeMG_ref.cpp ComputeMG.cpp ComputeProlongation_ref.cpp ComputeRestriction_ref.cpp GenerateCoarseProblem.cpp
    ComputeOptimalShapeXYZ.cpp MixedBaseCounter.cpp CheckAspectRatio.cpp OutputFile.cpp
    main.cpp
)

add_executable(xhpcg ${SOURCES})

target_compile_definitions(xhpcg PRIVATE -DHPCG_NO_MPI)

if (CMAKE_SYSTEM_NAME STREQUAL "Emu1")
    target_compile_definitions(xhpcg PRIVATE -DHPCG_NO_OPENMP)
else()
    find_package( OpenMP )

    if (OPENMP_FOUND)
        target_link_libraries(xhpcg PRIVATE "${OpenMP_CXX_FLAGS}")
        target_compile_options(xhpcg PRIVATE "${OpenMP_CXX_FLAGS}")
    else()
        target_compile_definitions(xhpcg PRIVATE -DHPCG_NO_OPENMP)
    endif()
endif()