file(GLOB PRESET_YAML "${PROJECT_SOURCE_DIR}/../brise/preset/*.yaml")
file(GLOB SUPPLEMENT_YAML "${PROJECT_SOURCE_DIR}/../brise/supplement/*.yaml")
file(GLOB EXTRA_YAML "${PROJECT_SOURCE_DIR}/../brise/extra/*.yaml")

set(GEN_RIME_DATADIR "${CMAKE_CURRENT_BINARY_DIR}/_data/rime")

add_custom_target(
    data
    ALL DEPENDS
    COMMAND ${CMAKE_COMMAND} -E make_directory ${GEN_RIME_DATADIR}
    COMMAND ${CMAKE_COMMAND} -E copy "${PROJECT_SOURCE_DIR}/../brise/default.yaml" ${GEN_RIME_DATADIR}
    COMMAND ${CMAKE_COMMAND} -E copy "${PROJECT_SOURCE_DIR}/../brise/essay.kct" ${GEN_RIME_DATADIR}
    COMMAND cp ${PRESET_YAML} ${GEN_RIME_DATADIR}
    COMMAND cp ${SUPPLEMENT_YAML} ${GEN_RIME_DATADIR}
    COMMAND cp ${EXTRA_YAML} ${GEN_RIME_DATADIR}
    COMMAND cd ${GEN_RIME_DATADIR} && "${CMAKE_CURRENT_BINARY_DIR}/../src/rime_deployer"
    COMMAND ${CMAKE_COMMAND} -E remove -f "${GEN_RIME_DATADIR}/rime.log" "${GEN_RIME_DATADIR}/installation.yaml"
)

install(DIRECTORY "${GEN_RIME_DATADIR}" DESTINATION ${FCITX4_PREFIX}/share/fcitx)
install(FILES zhung.png RENAME rime.png DESTINATION ${FCITX4_PREFIX}/share/fcitx/skin/default)
install(FILES zhung.png RENAME rime.png DESTINATION ${FCITX4_PREFIX}/share/fcitx/skin/dark)
install(FILES zhung.png RENAME rime.png DESTINATION ${FCITX4_PREFIX}/share/fcitx/skin/classic)
install(FILES zhung.png RENAME fcitx-rime.png DESTINATION share/icons/hicolor/48x48/apps)