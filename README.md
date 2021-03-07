# mysql_sys.io_global_by_file_by_bytes-

SELECT 
    `sys`.`format_path`(`performance_schema`.`file_summary_by_instance`.`FILE_NAME`) AS `file`,
    `performance_schema`.`file_summary_by_instance`.`COUNT_READ` AS `count_read`,
    FORMAT_BYTES(`performance_schema`.`file_summary_by_instance`.`SUM_NUMBER_OF_BYTES_READ`) AS `total_read`,
    FORMAT_BYTES(IFNULL((`performance_schema`.`file_summary_by_instance`.`SUM_NUMBER_OF_BYTES_READ` / NULLIF(`performance_schema`.`file_summary_by_instance`.`COUNT_READ`,
                            0)),
                    0)) AS `avg_read`,
    `performance_schema`.`file_summary_by_instance`.`COUNT_WRITE` AS `count_write`,
    FORMAT_BYTES(`performance_schema`.`file_summary_by_instance`.`SUM_NUMBER_OF_BYTES_WRITE`) AS `total_written`,
    FORMAT_BYTES(IFNULL((`performance_schema`.`file_summary_by_instance`.`SUM_NUMBER_OF_BYTES_WRITE` / NULLIF(`performance_schema`.`file_summary_by_instance`.`COUNT_WRITE`,
                            0)),
                    0.00)) AS `avg_write`,
    FORMAT_BYTES((`performance_schema`.`file_summary_by_instance`.`SUM_NUMBER_OF_BYTES_READ` + `performance_schema`.`file_summary_by_instance`.`SUM_NUMBER_OF_BYTES_WRITE`)) AS `total`,
    IFNULL(ROUND((100 - ((`performance_schema`.`file_summary_by_instance`.`SUM_NUMBER_OF_BYTES_READ` / NULLIF((`performance_schema`.`file_summary_by_instance`.`SUM_NUMBER_OF_BYTES_READ` + `performance_schema`.`file_summary_by_instance`.`SUM_NUMBER_OF_BYTES_WRITE`),
                            0)) * 100)),
                    2),
            0.00) AS `write_pct`
FROM
    `performance_schema`.`file_summary_by_instance`
ORDER BY (`performance_schema`.`file_summary_by_instance`.`SUM_NUMBER_OF_BYTES_READ` + `performance_schema`.`file_summary_by_instance`.`SUM_NUMBER_OF_BYTES_WRITE`) DESC
