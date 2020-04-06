---
title: mysql example extract mysql gzip (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'extract mysql gzip'


Modules used in program: 
* `import dataduct`

## python extract mysql gzip

Python mysql example: extract mysql gzip

```python
"""
ETL step wrapper to extract data from mysql to S3, compressing it along the way
"""
import dataduct
from dataduct.config import Config
from dataduct.steps.etl_step import ETLStep
from dataduct.pipeline import CopyActivity
from dataduct.pipeline import MysqlNode
from dataduct.pipeline import PipelineObject
from dataduct.pipeline import Precondition
from dataduct.pipeline import ShellCommandActivity
from dataduct.utils.helpers import exactly_one
from dataduct.utils.exceptions import ETLInputError
from dataduct.database import SelectStatement

config = Config()
if not hasattr(config, 'mysql'):
    raise ETLInputError('MySQL config not specified in ETL')

MYSQL_CONFIG = config.mysql

class ExtractMysqlGzipStep(ETLStep):
    """Extract Redshift Step class that helps get data out of redshift
    """

    def __init__(self,
                 table=None,
                 sql=None,
                 host_name=None,
                 database=None,
                 output_path=None,
                 splits=1,
                 gzip=False,
                 **kwargs):
        """Constructor for the ExtractMysqlGzipStep class

        Args:
            schema(str): schema from which table should be extracted
            table(path): table name for extract
            insert_mode(str): insert mode for redshift copy activity
            database(MysqlNode): database to excute the query
            splits(int): Number of files to split the output to.
            **kwargs(optional): Keyword arguments directly passed to base class
        """
        if not exactly_one(table, sql):
            raise ETLInputError('Only one of table, sql needed')

        super(ExtractMysqlGzipStep, self).__init__(**kwargs)

        if table:
            sql = 'SELECT * FROM %s;' % table
        elif sql:
            table = SelectStatement(sql).dependencies[0]
        else:
            raise ETLInputError('Provide a sql statement or a table name')

        host = MYSQL_CONFIG[host_name]['HOST']
        user = MYSQL_CONFIG[host_name]['USERNAME']
        password = MYSQL_CONFIG[host_name]['PASSWORD']

        input_node = self.create_pipeline_object(
            object_class=MysqlNode,
            schedule=self.schedule,
            host=host,
            database=database,
            table=table,
            username=user,
            password=password,
            sql=sql,
        )

        s3_format = self.create_pipeline_object(
            object_class=PipelineObject,
            type='TSV'
        )

        ### so - here's what's happening. DP does not like to encrypt if your next task is not Redshift
        ### so, i tricked it. Disconnected the s3data node from the cycle, and added a "fake" one
        ### that points to the same path. The fake one has a precondition (which appears to use polling)
        ### to ensure the path exists before attempting to run.
        compression = "none"
        if gzip:
            compression = "gzip"

        intermediate_node = self.create_s3_data_node(format=s3_format, compression=compression)
        
        fake_intermediate_node = None

        ### if we're not gzip, let's conserve number of objects created
        if gzip: 
            precondition = self.create_pipeline_object(
                    object_class=Precondition,
                    is_directory=True
            )
            fake_intermediate_node = self.create_s3_data_node(format=s3_format, precondition=precondition) 

            ### explicitly set the directory path to that of the intermediate_node
            fake_intermediate_node.fields['directoryPath'][0] = intermediate_node.fields['directoryPath'][0]

        self.create_pipeline_object(
            object_class=CopyActivity,
            schedule=self.schedule,
            resource=self.resource,
            worker_group=self.worker_group,
            input_node=input_node,
            output_node=intermediate_node,
            depends_on=self.depends_on,
            max_retries=self.max_retries,
        )

        self._output = self.create_s3_data_node(
            self.get_output_s3_path(output_path))

        gunzip_part = \
          "for file in ${INPUT1_STAGING_DIR}/*.gz; do gunzip -f ${file}; done; " \
          if gzip else ""

        gzip_part = "; for file in ${OUTPUT1_STAGING_DIR}/*; do gzip -f $file; done" \
          if gzip else ""



        # This shouldn't be necessary but -
        # Mysql uses \\n as null, so we need to remove it
        command = ' '.join(["[[ -z $(find ${INPUT1_STAGING_DIR} -maxdepth 1 ! \
                           -path ${INPUT1_STAGING_DIR} -name '*' -size +0) ]] \
                           && touch ${OUTPUT1_STAGING_DIR}/part-0 ",
                           "|| ",
                           gunzip_part,
                           "cat ${INPUT1_STAGING_DIR}/*",
                           "| sed 's/\\\\\\\\n/NULL/g'",  # replace \\n
                           # get rid of control characters
                           "| tr -d '\\\\000'",
                           # split into `splits` number of equal sized files
                           ("| split -a 4 -d -l $((($(cat ${{INPUT1_STAGING_DIR}}/* | wc -l) + \
                           {splits} - 1) / {splits})) - ${{OUTPUT1_STAGING_DIR}}/part-"). \
                           format(splits=splits),
                           "; for f in ${INPUT1_STAGING_DIR}/*; do echo ${f}; file ${f}; done ",
                           gzip_part])

        input_node = fake_intermediate_node if gzip else intermediate_node
        self.create_pipeline_object(
            object_class=ShellCommandActivity,
            input_node=input_node,
            output_node=self.output,
            command=command,
            max_retries=self.max_retries,
            resource=self.resource,
            worker_group=self.worker_group,
            schedule=self.schedule,
        )

    @classmethod
    def arguments_processor(cls, etl, input_args):
        """Parse the step arguments according to the ETL pipeline

        Args:
            etl(ETLPipeline): Pipeline object containing resources and steps
            step_args(dict): Dictionary of the step arguments for the class
        """
        input_args = cls.pop_inputs(input_args)
        step_args = cls.base_arguments_processor(etl, input_args)

        return step_args


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
