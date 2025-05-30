import argparse  
import os  
import sys  
  
import django  
  
parser = argparse.ArgumentParser()  
parser.add_argument("-action", "-a", help="Action")  # Действие  
parser.add_argument('-cars', type=int, help='amount_cars')  
parser.add_argument('-id', type=int)  
parser.add_argument('-proxy', type=int)  
args = parser.parse_args()  
print(args)  
  
if __name__ == "__main__":  
    if len(sys.argv) > 1:  
        os.environ.setdefault("DJANGO_SETTINGS_MODULE", "project.settings")  
        django.setup()  
        from fines.utils import *  
        from passes.utils import *  
        # Эта команда запускает скрип в файле utils.py  
        if args.action == "get_fines":  
            get_fines(args.cars, args.id)  
  
        if args.action == 'downloads_image':    # загрузить фотографии получиться лишь в течении часа после обновления штрафов  
            downloads_image()  
  
        if args.action == 'download_tickets':  
            test_ticket(args.cars, args.id, args.proxy)  
  
        if args.action == 'download_tickets_async':  
            create_passes_async()  
  
    else:  
        print("Нет параметров!")  
  
  
-a update_checks -f_car 1 -to_car 10000 -id 1