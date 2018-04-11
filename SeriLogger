using Serilog;
using Serilog.Events;
using Serilog.Sinks.MSSqlServer;
using System;
using System.Collections.ObjectModel;
using System.Data;

namespace Logger
{
    public class SeriLogger
    {
        private static readonly ILogger _Logger;
        private static readonly object padlock = new object();

        private static SeriLogger instance = null;

        public static SeriLogger Instance
        {
            get
            {
                if (instance == null)
                {
                    lock (padlock)
                    {
                        if (instance == null)
                        {
                            instance = new SeriLogger();
                        }
                    }
                }
                return instance;
            }
        }

        static SeriLogger()
        {
            var connStr = "Data Source=LAPTOP-B7TENUPP;Initial Catalog=Logger;Integrated Security=True;";
            if (_Logger == null)
            {
                _Logger = new LoggerConfiguration()
                            .WriteTo.MSSqlServer(connStr, "DMECSLog", columnOptions: GetSqlColumnOptions())
                            .CreateLogger();
            }

        }

        public static ColumnOptions GetSqlColumnOptions()
        {
            var colOptions = new ColumnOptions();
            colOptions.Store.Remove(StandardColumn.MessageTemplate);

            colOptions.AdditionalDataColumns = new Collection<DataColumn>
            {
                new DataColumn{DataType = typeof(string), ColumnName = "UserCode"},
                new DataColumn{DataType = typeof(string), ColumnName = "AppName"},
                new DataColumn{DataType = typeof(string), ColumnName = "FileName"},
                new DataColumn{DataType = typeof(string), ColumnName = "FunctionName"}
            };
            colOptions.Properties.ExcludeAdditionalProperties = true;
            return colOptions;
        }


        public void Debug(string userCode, string appName, string fileName, string functionName, string message)
        {
            _Logger.ForContext("UserCode", userCode)
                .ForContext("AppName", appName)
                .ForContext("FileName", fileName)
                .ForContext("FunctionName", functionName).Debug(message);
        }
        public void Information(string userCode, string appName, string fileName, string functionName, string message)
        {
            _Logger.ForContext("UserCode", userCode)
                .ForContext("AppName", appName)
                .ForContext("FileName", fileName)
                .ForContext("FunctionName", functionName).Information(message);
        }
        public void Warning(string userCode, string appName, string fileName, string functionName, string message)
        {
            _Logger.ForContext("UserCode", userCode)
                .ForContext("AppName", appName)
                .ForContext("FileName", fileName)
                .ForContext("FunctionName", functionName).Warning(message);
        }
        public void Fatal(string userCode, string appName, string fileName, string functionName, Exception exception)
        {
            _Logger.ForContext("UserCode", userCode)
                .ForContext("AppName", appName)
                .ForContext("FileName", fileName)
                .ForContext("FunctionName", functionName).Fatal(exception,exception.Message);
        }
        public void Verbose(string userCode, string appName, string fileName, string functionName, Exception exception)
        {
            _Logger.ForContext("UserCode", userCode)
                .ForContext("AppName", appName)
                .ForContext("FileName", fileName)
                .ForContext("FunctionName", functionName).Verbose(exception,exception.Message);
        }

        public void Error(string userCode, string appName, string fileName, string functionName, Exception exception)
        {
            _Logger.ForContext("UserCode", userCode)
                .ForContext("AppName", appName)
                .ForContext("FileName", fileName)
                .ForContext("FunctionName", functionName).Error(exception,exception.Message);
        }
    }
}
